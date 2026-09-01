# OpenCode

## Purpose

This page documents my OpenCode environment and workflow.

The goal is not to document OpenCode itself, but rather how I use it alongside tmux, Neovim, Git, and ChatGPT.

The OpenCode documentation is the source of truth for every feature and command. This page records the conventions, workflow, and important concepts I am likely to forget.

---

## Philosophy

OpenCode is an AI coding assistant that operates directly inside a project repository.

The goal is to use it as an **AI-assisted development tool**, not as a replacement for understanding the codebase.

OpenCode should help with:

- repository-aware planning
- codebase exploration
- implementation
- repetitive edits
- debugging
- refactoring
- testing
- documentation
- research related to the current project

I should still remain responsible for:

- architecture
- product decisions
- implementation direction
- code review
- understanding changes
- deciding what belongs in the codebase
- determining whether the result is actually good

The goal is to gain leverage without becoming a passive diff reviewer.

---

## Division of Labor

### ChatGPT

Use ChatGPT primarily as the broad thinking and planning layer.

Typical uses:

- product ideas
- architecture discussions
- design critique
- requirements
- tradeoffs
- research
- debugging strategy
- turning vague ideas into implementation plans
- writing detailed prompts/briefs for OpenCode
- discussing things that span multiple projects or are not primarily coding tasks

ChatGPT is the **whiteboard**.

### OpenCode

Use OpenCode when repository context matters.

Typical uses:

- inspect the existing implementation
- determine where a feature belongs
- plan repository-specific changes
- implement features
- refactor code
- run tests
- investigate bugs
- inspect dependencies
- make coordinated changes across files

OpenCode is the **repo-native development assistant**.

---

## Project Model

### One tmux Session Per Project

Each development project gets its own tmux session.

Example:

```text
tmux session: langlife

1: nvim
2: shell
3: server
4: opencode
```

OpenCode should normally be launched from the root of the individual project:

```bash
cd ~/code/langlife
opencode
```

Do not normally launch OpenCode from:

```bash
~/code
```

because that exposes multiple unrelated repositories as one workspace.

---

## OpenCode Sessions

An OpenCode **session** is a conversation/context thread inside a project.

Sessions should normally map to a coherent feature, bug, or task rather than one permanent conversation for the entire project.

Example:

```text
LangLife

- Marketing site
- Async Auto Send
- Study loop
- Android capture UX
- Authentication cleanup
- Location-aware review
```

This provides both:

- focused context
- durable history for that particular feature

### Start a New Session

```text
/new
```

Default keybind:

```text
Ctrl-X n
```

### List / Resume Sessions

```text
/sessions
```

Aliases:

```text
/resume
/continue
```

Default keybind:

```text
Ctrl-X l
```

Use an existing session when returning to the same feature or problem.

Start a new session when moving to a meaningfully different piece of work.

---

## Persistent Project Knowledge: AGENTS.md

Session context should contain information specific to the current task.

Long-lived project knowledge belongs in:

```text
AGENTS.md
```

This is one of the most important OpenCode concepts.

OpenCode automatically loads `AGENTS.md` as project instructions for future sessions.

Typical contents include:

- project architecture
- build commands
- test commands
- lint commands
- important domain concepts
- unusual conventions
- repository structure
- setup quirks
- operational gotchas
- verification requirements

### Create or Update AGENTS.md

Inside OpenCode:

```text
/init
```

`/init` scans important repository files and creates or updates `AGENTS.md`.

Review the generated file before committing it.

Commit the project-level `AGENTS.md` to Git.

Example:

```text
langlife/
├── AGENTS.md
├── app/
├── components/
└── ...
```

### Mental Model

```text
AGENTS.md
    =
persistent project knowledge

OpenCode session
    =
current feature/problem knowledge
```

This distinction is fundamental.

A new session should not need to rediscover the entire project architecture.

It should receive the durable project knowledge from `AGENTS.md`, then inspect only the files relevant to the task.

---

## Global AGENTS.md

OpenCode also supports global instructions:

```text
~/.config/opencode/AGENTS.md
```

Use this for personal development rules that should apply across projects.

Potential examples:

```markdown
# Development Preferences

- Prefer simple solutions over unnecessary abstraction.
- Follow existing project conventions before introducing new patterns.
- Do not add dependencies without a clear reason.
- Do not modify unrelated files.
- Explain architectural changes before implementing them.
- Run relevant tests after changes.
- Keep changes scoped to the current task.
- Prefer readable code over clever code.
```

Keep global rules generic.

Project-specific knowledge belongs in the repository's `AGENTS.md`.

---

## Plan vs Build

OpenCode has two primary agents:

```text
Plan
Build
```

Switch between primary agents with:

```text
Tab
```

### Plan

Use **Plan** when deciding what should be done.

Typical uses:

- inspect architecture
- investigate a bug
- determine affected files
- propose an implementation
- compare approaches
- review a feature before editing
- understand unfamiliar code

Plan is intentionally restricted compared with Build.

Use Plan for non-trivial changes before allowing implementation.

Example workflow:

```text
Plan
  ↓
inspect repository
  ↓
propose implementation
  ↓
review / revise
  ↓
Build
```

### Build

Use **Build** when ready to make changes.

Build has access to file editing and command execution according to configured permissions.

Typical uses:

- create files
- edit files
- implement features
- refactor code
- run tests
- fix compile errors
- make coordinated changes

Do not use Build merely because it is faster when the implementation direction is still unclear.

---

## Permissions

OpenCode permissions control what the agent may do automatically.

Permissions can be:

```text
allow
ask
deny
```

For my workflow, the preferred philosophy is:

```text
read/search → allow
edit        → ask
bash        → ask
```

This allows OpenCode to freely understand the project while keeping actual changes under review.

Example configuration:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "read": "allow",
    "glob": "allow",
    "grep": "allow",
    "lsp": "allow",
    "edit": "ask",
    "bash": "ask"
  }
}
```

This will likely be slower than fully autonomous operation.

That is intentional.

The goal is to retain awareness of what is changing and continue actively exercising development skills.

Autonomy can be loosened later for operations that become proven and low-risk.

---

## Subagents

Subagents are specialized child agents that can investigate pieces of a task without filling the primary session with all of their exploratory context.

This is important for both context efficiency and parallel work.

Conceptually:

```text
Primary Agent
    │
    ├── Explore repository area A
    │
    ├── Investigate repository area B
    │
    └── Research dependency C
             │
             ▼
       condensed findings
             │
             ▼
       Primary Agent
```

Each subagent operates in its own child session with fresh context.

This means the primary agent can receive conclusions instead of carrying every file read, search result, and intermediate investigation in its own context.

### Built-in Subagents

#### Explore

Read-only repository exploration.

Useful for:

- finding files
- searching code
- understanding a subsystem
- tracing implementations
- answering repository questions

#### General

General-purpose multi-step work and research.

Useful for larger delegated investigations.

#### Scout

External dependency and documentation research.

Useful when the answer requires looking outside the local repository.

### Manual Invocation

Subagents can be explicitly referenced using `@`.

Example:

```text
@explore find where OCR tokens are converted into learning items
```

Primary agents can also invoke appropriate subagents automatically.

---

## Context Windows

The active context window is primarily determined by the selected AI model.

OpenCode cannot make a model's native context window arbitrarily larger.

Context includes things such as:

- prompts
- replies
- instructions
- file contents
- command output
- tool results
- repository exploration
- reasoning history

The percentage displayed in OpenCode represents how much of the current session's context window is being used.

Do not confuse this with API spending.

---

## Context Compaction

OpenCode automatically supports **compaction** when a session becomes large.

Compaction replaces older active context with a structured summary/checkpoint while preserving recent context.

This frees context space while allowing the session to continue.

Manual command:

```text
/compact
```

Alias:

```text
/summarize
```

Default keybind:

```text
Ctrl-X c
```

Compaction is useful, but it is lossy.

Important subtle details can disappear when old conversation history is summarized.

Therefore:

- do not depend on one immortal session
- start new sessions for separate features
- store durable architecture and conventions in `AGENTS.md`

Preferred pattern:

```text
feature
  ↓
session
  ↓
finish feature
  ↓
new feature
  ↓
new session
```

Use compaction to continue long coherent tasks, not as a substitute for good session boundaries.

---

## File References

Files can be added explicitly to prompts using:

```text
@
```

Example:

```text
Explain how authentication works in @app/composables/useAuth.ts
```

OpenCode performs fuzzy file search and adds the referenced file to the conversation.

Use explicit references when the relevant file is already known rather than asking the agent to search unnecessarily.

---

## Images

Images can be attached to prompts.

Useful for:

- marketing-site references
- UI comparisons
- screenshots of bugs
- design mockups
- responsive issues

An image can be dragged into the terminal and attached to the current prompt.

When possible, keep design reference images inside a project directory so they can be reused during implementation.

Example:

```text
design/
├── langlife-medium.png
├── langlife-max.png
└── final-direction.png
```

---

## Shell Commands

A shell command can be run from the OpenCode prompt by prefixing it with:

```text
!
```

Example:

```text
!git status
```

The command output becomes part of the conversation.

Use normal tmux shell windows for ordinary shell work.

Use `!` when the command output should become context for the current OpenCode discussion.

---

## Undo / Redo

OpenCode tracks file changes using snapshots when working inside a Git repository.

### Undo

```text
/undo
```

Default keybind:

```text
Ctrl-X u
```

This removes the most recent conversation step and reverts associated file changes.

### Redo

```text
/redo
```

Default keybind:

```text
Ctrl-X r
```

This restores the undone step and file changes.

Snapshots are extremely useful for experimentation.

They are **not a replacement for Git**.

Commands may modify things that snapshots cannot safely restore, including:

- databases
- external services
- ignored files
- Git metadata
- remote resources

Continue using normal Git commits at meaningful milestones.

---

## Git Workflow

Do not create a Git commit before every single OpenCode prompt.

Instead:

```text
clean working tree
    ↓
start feature session
    ↓
Plan
    ↓
Build
    ↓
review proposed edits
    ↓
test
    ↓
use /undo if necessary
    ↓
commit meaningful working milestone
```

Git remains the authoritative version-control system.

OpenCode snapshots provide convenient short-term experimentation and reversal.

---

## Models and Reasoning Levels

OpenCode supports multiple AI providers and models.

Model selection should depend on the task.

Current working philosophy:

```text
Normal implementation
    → medium reasoning

Routine fixes / compile errors
    → medium reasoning

Architecture / difficult debugging
    → higher reasoning if useful

Initial visual concept / difficult high-leverage design
    → experiment with higher reasoning

Routine implementation after concept is established
    → return to medium
```

Higher reasoning is not automatically better.

Testing with LangLife marketing concepts showed that a higher reasoning level could produce stronger content and explanation while a medium reasoning level produced a more restrained and artistic visual design.

Use expensive reasoning intentionally rather than assuming maximum reasoning should always be selected.

OpenCode model variants can be cycled using:

```text
Ctrl-T
```

Use:

```text
/models
```

to select available models.

---

## Cost vs Context

These are separate concepts.

```text
Context %
    =
how full the current model context is

Cost
    =
estimated API usage generated by the session
```

A large context window does not mean that percentage of the monthly API budget has been spent.

Use the provider's billing dashboard as the authoritative source for account spending.

OpenCode's cost display is useful as a local estimate.

---

## Recommended Feature Workflow

For a meaningful new feature:

### 1. Start a New Session

```text
/new
```

Give the session one coherent responsibility.

### 2. Use Plan

Explain the goal.

Have OpenCode:

- inspect relevant architecture
- find existing patterns
- identify files
- propose the approach
- point out risks
- describe verification

Review the plan before proceeding.

### 3. Switch to Build

```text
Tab
```

Ask OpenCode to implement the agreed plan.

With edit permissions set to `ask`, review modifications before applying them.

### 4. Review the Code

Do not accept a change merely because:

- it compiles
- tests pass
- the agent sounds confident

Understand:

- what changed
- why it changed
- whether it follows existing architecture
- whether unnecessary abstractions were introduced
- whether unrelated files changed

### 5. Test

Run the appropriate:

- unit tests
- integration tests
- type checking
- linting
- build
- manual verification

### 6. Commit

Once the feature reaches a meaningful working state:

```bash
git add .
git commit
```

Then move on to the next coherent task.

---

## Recommended Session Boundaries

Good session:

```text
Implement async Auto Send
```

Good session:

```text
Investigate authentication redirect loop
```

Good session:

```text
Build LangLife marketing site
```

Bad session:

```text
LangLife development forever
```

If a task would normally deserve its own branch, issue, feature description, or focused work block, it probably deserves its own OpenCode session.

---

## Design Workflow

For visual / marketing work:

```text
ChatGPT
    ↓
product direction / design critique
    ↓
visual concept
    ↓
OpenCode Plan
    ↓
translate design into repo implementation
    ↓
OpenCode Build
    ↓
implement
    ↓
browser review
    ↓
ChatGPT + personal critique
    ↓
refinement pass
```

OpenCode is particularly useful once there is a concrete design target because it can modify the actual implementation without requiring code to be copied back and forth.

For initial design exploration, multiple model variants may be worth testing independently.

Preserve good ideas across versions rather than automatically choosing the version generated with the highest reasoning setting.

---

## Useful Commands

| Command | Purpose |
| --- | --- |
| `opencode` | Start OpenCode in current directory |
| `/help` | Show available commands |
| `/init` | Create/update `AGENTS.md` |
| `/new` | Start fresh session |
| `/sessions` | List/resume sessions |
| `/models` | Select model |
| `/compact` | Compact current session |
| `/undo` | Undo last conversation step and file changes |
| `/redo` | Restore undone step |
| `/details` | Toggle tool execution details |
| `/editor` | Compose prompt in external editor |
| `/export` | Export session as Markdown |
| `/exit` | Exit OpenCode |
| `@file` | Add file to prompt context |
| `!command` | Run shell command and add output to context |
| `Tab` | Switch primary agent (Plan / Build) |
| `Ctrl-T` | Cycle model reasoning variants |

---

## Important Things to Remember

### Project Knowledge and Feature Knowledge Are Different

```text
AGENTS.md
    =
what every agent working on this project should know

Session
    =
what the agent working on this particular task should know
```

### New Sessions Are Cheap

Do not preserve a bloated session simply because it contains history.

If the task has changed, start a new session.

The codebase and `AGENTS.md` already provide persistent project context.

### Let Subagents Explore

Do not assume every repository investigation needs to consume the primary context window.

Subagents can investigate areas independently and return condensed findings.

### Plan Before Large Changes

If the change involves architecture, multiple systems, or unclear requirements:

```text
Plan first.
```

Build only after the direction is understood.

### AI Should Save Typing, Not Eliminate Understanding

The agent may write the implementation.

I still need to understand and own the implementation.

The goal is:

```text
AI writes faster
+
I continue thinking
```

not:

```text
AI thinks
+
I approve
```

---

## Configuration Locations

Global OpenCode configuration:

```text
~/.config/opencode/
```

Important files may include:

```text
~/.config/opencode/AGENTS.md
~/.config/opencode/opencode.json
~/.config/opencode/tui.json
~/.config/opencode/agents/
~/.config/opencode/commands/
```

Project-specific configuration may live inside:

```text
.opencode/
```

Project instructions:

```text
AGENTS.md
```

---

## Future Improvements

Potential things to configure once real usage reveals friction:

- global permission defaults
- OpenCode-specific tmux startup
- custom keyboard shortcuts
- custom agents
- reusable commands
- project-specific OpenCode configuration
- specialized review agent
- specialized test agent
- automatic project/session launch helpers
- tighter integration with Neovim
- shell aliases
- better project bootstrap scripts

Do not configure these merely because they exist.

Add them only when actual usage reveals friction.