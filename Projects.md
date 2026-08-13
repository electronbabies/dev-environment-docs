# Projects

## Purpose

All development projects live under one canonical root:

```text
~/code
```

Do not scatter active projects across `~/Projects`, `~/AndroidStudioProjects`, or other application-specific directories.

The goal is to make every project easy to find, audit, back up, and return to later.

## Current Layout

```text
~/code/
├── dotfiles/
├── kanji-analyzer/
├── langlife/
│   ├── android/
│   ├── api/
│   └── ui/
└── sugacoded/
```

LangLife is one product with three separate Git repositories, so its repositories are grouped under one product directory.

`kanji-analyzer` is also version controlled. Its `reference/` directory contains the original PHP implementation used as a reference while rewriting the analyzer in Kotlin.

## Rule

`~/code` is the canonical home for development projects and development tooling.

Not every Git repository must physically live under `~/code`. Repositories that belong elsewhere for a good semantic reason can be explicitly registered with the repository checkpoint.

Current explicit exception:

```text
~/Nextcloud/ObsidianVault/Infrastructure
```

The Infrastructure repository remains inside the Nextcloud-backed Obsidian vault because it is documentation, not a software project. It is separately version controlled so the infrastructure documentation can also be published through GitHub.

Git repositories may be nested below product directories. The repository checkpoint discovers repositories recursively under `~/code` and also checks explicitly configured exceptions.

---

# Repository Checkpoint

Use `repos` as the central checkpoint for Git state across all repositories that matter to the development environment.

It recursively discovers repositories under `~/code` and also includes explicitly configured repositories outside that tree.

```bash
repos
```

The command reports repository path, branch, working-tree state, and remote synchronization state. It also catches missing remotes/upstreams and ahead, behind, or diverged branches.

The script is read-only. It does not commit, pull, merge, or push anything.

## Fast Local Check

```bash
repos
```

This does not contact remotes. Use it frequently to answer:

> Did I leave modified files, untracked files, or unpushed commits in another repository?

Remote status is based on the remote information Git already knows locally.

## Fresh Remote Check

```bash
repos --fetch
```

This runs `git fetch --quiet --prune` before calculating remote state.

Use it when another developer, another computer, GitHub, or automation may have changed the remote, or when I want an authoritative synchronization check.

`git fetch` updates local knowledge of the remote. It does not merge remote work into the current branch or modify working files.

## Help

```bash
repos --help
```

## Location and Configuration

```text
~/code/dotfiles/bin/repos
```

`~/code/dotfiles/bin` is on `PATH`, so the command can be run simply as `repos`.

The script's `EXTRA_REPOS` array contains intentional repositories outside `~/code`, currently:

```text
~/Nextcloud/ObsidianVault/Infrastructure
```

Do not move a repository into `~/code` merely to satisfy the audit tool. Keep the filesystem organized by purpose and configure the checkpoint to reflect reality.

## End-of-Session Habit

Run:

```bash
repos
```

Resolve anything unexpectedly dirty or ahead of its remote.

When fresh remote comparison matters:

```bash
repos --fetch
```

The goal is one place to answer:

> Is there anything anywhere that I forgot to commit or push?


---

# Engineering Rationale

The organization and repository checkpoint are intended to reduce dependence on memory.

The system should make important state visible rather than requiring me to remember which projects I touched, where they live, or whether each one was committed and pushed.

Principles demonstrated by this setup:

- **Reproducibility:** development configuration and personal tooling live in version-controlled dotfiles and can be reinstalled.
- **Visibility:** `repos` exposes repository state from one checkpoint instead of requiring manual checks in every project.
- **Failure prevention:** the workflow catches forgotten changes and unpushed commits before they disappear into an old project directory.
- **Organization by purpose:** software projects live under `~/code`, while legitimate exceptions such as the Obsidian Infrastructure documentation remain where they semantically belong.
- **Prefer systems over memory:** recurring checks should be automated when practical instead of relying on remembering to perform the same manual process repeatedly.

When documenting future environment tooling, record not only the commands but also the failure mode or workflow problem the tooling is intended to prevent.
