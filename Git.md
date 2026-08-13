# Git

Practical Git reference for everyday development, repository maintenance, branching, merging, rebasing, and recovery.

This is not intended to document every Git command. It focuses on commands and workflows that are useful during normal development and occasional repository surgery.

---

# Multi-Repository Checkpoint

`~/code` is the canonical home for development projects. The `repos` checkpoint also includes explicitly configured Git repositories that intentionally live elsewhere, such as the Infrastructure documentation inside the Nextcloud-backed Obsidian vault.

Use `repos` for the fast local checkpoint and `repos --fetch` when remote state must be refreshed first. The command is intentionally read-only.

Full workflow: [[Projects]]

---

# Mental Model

Git has several important areas to keep straight:

```text
Working Directory
      │
      │ git add
      ▼
Staging Area / Index
      │
      │ git commit
      ▼
Local Repository
      │
      │ git push
      ▼
Remote Repository
```

A file can therefore be:

```text
untracked
modified
staged
committed
pushed
```

Understanding which state a file is currently in makes most Git commands much easier to reason about.

---

# Repository Status

The most useful Git command:

```bash
git status
```

Use it constantly.

It shows:

- Current branch
- Modified files
- Staged changes
- Untracked files
- Branch relationship with the remote

A shorter version:

```bash
git status --short
```

Example:

```text
 M README.md
M  src/app.ts
?? notes.txt
```

The two columns represent staged and working-tree state.

---

# View Changes

Show unstaged changes:

```bash
git diff
```

Show staged changes:

```bash
git diff --staged
```

Show changes introduced by the most recent commit:

```bash
git show
```

Show a particular commit:

```bash
git show <commit>
```

---

# Staging Changes

Stage a file:

```bash
git add README.md
```

Stage several files:

```bash
git add README.md package.json src/app.ts
```

Stage everything in the current repository:

```bash
git add .
```

Stage changes interactively:

```bash
git add -p
```

`-p` means **patch**.

This lets individual hunks of a file be staged separately.

Useful when unrelated changes accidentally ended up in the same file but should belong to different commits.

---

# Commits

Create a commit:

```bash
git commit -m "Add project navigation"
```

Inspect recent commits:

```bash
git log
```

A compact history:

```bash
git log --oneline
```

A useful visual history:

```bash
git log --oneline --graph --decorate --all
```

Example:

```text
* 1b3c482 (HEAD -> main) Add project navigation
* a8e2201 Update dependencies
* 41c9210 Initial commit
```

---

# Amend the Last Commit

If the most recent commit is missing something:

```bash
git add forgotten-file
git commit --amend
```

To amend without changing the commit message:

```bash
git commit --amend --no-edit
```

Example:

```bash
git add README.md
git commit --amend --no-edit
```

## Important

Amending creates a **new commit** that replaces the previous one.

Avoid casually amending commits that have already been pushed and shared with other developers.

---

# Branches

List local branches:

```bash
git branch
```

List local and remote branches:

```bash
git branch -a
```

Create a branch:

```bash
git branch redesign
```

This creates the branch but does **not** switch to it.

---

# `git switch`

Modern Git provides `git switch` specifically for changing branches.

Switch branches:

```bash
git switch redesign
```

Create a new branch and immediately switch to it:

```bash
git switch -c redesign
```

`-c` means **create**.

Equivalent older syntax:

```bash
git checkout redesign
```

and:

```bash
git checkout -b redesign
```

Prefer:

```bash
git switch
```

for branch operations because the intent is clearer.

---

# Why `switch` Exists

Historically, `git checkout` performed several unrelated jobs.

It could switch branches:

```bash
git checkout main
```

but could also restore files:

```bash
git checkout -- README.md
```

Modern Git separates these concepts:

```text
git switch    → branches
git restore   → files
```

Examples:

```bash
git switch main
```

and:

```bash
git restore README.md
```

This makes potentially destructive commands easier to understand.

---

# Restore Files

Discard unstaged changes to a file:

```bash
git restore README.md
```

Discard unstaged changes to everything:

```bash
git restore .
```

## Warning

This destroys uncommitted working-directory changes.

Check first:

```bash
git status
git diff
```

---

# Unstage a File

If a file has been staged accidentally:

```bash
git restore --staged README.md
```

The file remains modified in the working directory, but is removed from the staging area.

For everything:

```bash
git restore --staged .
```

---

# Remove Files With Git

Remove a tracked file:

```bash
git rm file.txt
```

This:

1. Removes the file from the working directory.
2. Stages its deletion.

Equivalent to manually deleting it and then staging the deletion.

---

# Remove All Tracked Files From a Repository

To remove all files currently tracked by Git while preserving the repository:

```bash
git rm -rf .
```

Here:

```text
-r    recursive
-f    force
```

This:

- Removes tracked files and directories.
- Stages their deletion.
- Preserves `.git`.
- Preserves the repository's history.
- Does **not** remove untracked files.
- Does **not** remove ignored files.

This is useful when completely replacing an existing project's contents while retaining the Git repository and history.

Example:

```bash
git switch -c redesign
git rm -rf .
```

Then inspect what remains:

```bash
git status
ls -la
```

## Example

Before:

```text
.git/
package.json
src/
README.md
.env
node_modules/
notes.txt
```

Assume:

```text
package.json    tracked
src/            tracked
README.md       tracked

.env            ignored
node_modules/   ignored
notes.txt       untracked
```

After:

```bash
git rm -rf .
```

the directory may contain:

```text
.git/
.env
node_modules/
notes.txt
```

Git only removed the tracked contents.

This makes:

```bash
git rm -rf .
```

considerably safer and more intentional than blindly using filesystem commands when the goal is specifically to remove a repository's tracked project files.

---

# Remove a File From Git but Keep It Locally

Sometimes a file was accidentally committed but should remain on the local machine.

Use:

```bash
git rm --cached filename
```

Example:

```bash
git rm --cached .env
```

Then add the file to:

```text
.gitignore
```

and commit the change.

For a directory:

```bash
git rm -r --cached directory/
```

---

# `.gitignore`

`.gitignore` prevents **untracked files** from being added to Git.

Example:

```gitignore
node_modules/
.env
.output/
.nuxt/
.DS_Store
```

## Important

Adding an already-tracked file to `.gitignore` does **not** make Git stop tracking it.

If `.env` was already committed:

```bash
git rm --cached .env
```

Then commit that deletion from Git.

---

# Remotes

List configured remotes:

```bash
git remote -v
```

Typical output:

```text
origin  git@github.com:user/project.git (fetch)
origin  git@github.com:user/project.git (push)
```

Add a remote:

```bash
git remote add origin <repository-url>
```

Change the URL:

```bash
git remote set-url origin <repository-url>
```

---

# Fetch vs Pull

These are related but not identical.

## Fetch

```bash
git fetch
```

Downloads information from the remote without modifying the current working branch.

Think:

```text
"Tell me what changed remotely."
```

This is useful when you want to inspect remote changes before integrating them.

Fetch everything:

```bash
git fetch --all
```

---

## Pull

```bash
git pull
```

Conceptually performs:

```text
fetch
+
integrate remote changes
```

The integration is usually a merge or rebase depending on configuration/options.

Think:

```text
"Get the remote changes and integrate them into my branch."
```

---

# Push

Push the current branch:

```bash
git push
```

For a new local branch that does not yet have an upstream:

```bash
git push -u origin redesign
```

`-u` / `--set-upstream` associates the local branch with the remote branch.

After that:

```bash
git push
git pull
```

can normally be used without specifying the remote and branch.

---

# Merge

Suppose history looks like:

```text
A---B---C main
     \
      D---E feature
```

While on `main`:

```bash
git merge feature
```

Git may produce:

```text
A---B---C-------M main
     \         /
      D-------E feature
```

`M` is a merge commit.

The history records that two development paths existed and were joined.

---

# Rebase

Starting with the same history:

```text
A---B---C main
     \
      D---E feature
```

While on `feature`:

```bash
git rebase main
```

Git conceptually takes:

```text
D
E
```

and replays those changes on top of `main`.

Result:

```text
A---B---C main
         \
          D'---E' feature
```

The commits become:

```text
D'
E'
```

because rebasing creates new commits.

The changes may be equivalent, but the commit identities are different.

---

# Merge vs Rebase

The simplest mental model:

```text
Merge:
"Combine these histories."

Rebase:
"Pretend my work started from the newer base."
```

## Merge

Advantages:

- Preserves actual branch history.
- Does not rewrite existing commits.
- Safe for shared branches.
- Clearly represents when branches diverged and merged.

Disadvantages:

- Can produce many merge commits.
- History can become visually noisy.

## Rebase

Advantages:

- Produces cleaner, more linear history.
- Makes feature work appear based on the latest branch state.
- Useful before merging a local feature branch.

Disadvantages:

- Rewrites commit history.
- Changes commit hashes.
- Dangerous when casually performed on shared/published history.

---

# Practical Rebase Workflow

Suppose:

```text
main
feature
```

and `main` has advanced while working on `feature`.

Update your knowledge of the remote:

```bash
git fetch origin
```

Switch to the feature:

```bash
git switch feature
```

Rebase onto current remote `main`:

```bash
git rebase origin/main
```

If there are no conflicts, the feature now sits cleanly on top of current `main`.

---

# The Golden Rebase Rule

A useful rule:

> **Do not rebase commits other people may already be building work on unless everyone involved understands and expects the history rewrite.**

Rebasing your own local feature branch is usually fine.

Rebasing a shared production branch casually is not.

---

# Rebase Conflicts

If Git encounters a conflict during a rebase, it stops.

Check:

```bash
git status
```

Resolve the conflicting files.

Then stage them:

```bash
git add <resolved-files>
```

Continue:

```bash
git rebase --continue
```

If the rebase has become a disaster:

```bash
git rebase --abort
```

This returns the repository to its pre-rebase state.

---

# Merge Conflicts

When a merge conflicts:

```bash
git status
```

Git identifies conflicting files.

Inside those files, conflicts may look like:

```text
<<<<<<< HEAD
current branch
=======
incoming branch
>>>>>>> feature
```

Edit the file into the desired final state and remove the markers.

Then:

```bash
git add resolved-file
```

Once all conflicts are resolved:

```bash
git commit
```

To abandon the merge:

```bash
git merge --abort
```

---

# Fast-Forward Merges

Sometimes Git does not need a merge commit.

Example:

```text
A---B main
     \
      C---D feature
```

If `main` has not changed since `feature` branched, merging can simply move `main` forward:

```text
A---B---C---D main
```

This is a **fast-forward merge**.

No additional merge commit is necessary.

---

# Delete Branches

Delete a local branch that Git considers safely merged:

```bash
git branch -d feature
```

Force deletion:

```bash
git branch -D feature
```

Use `-D` carefully because Git will delete the branch even if its commits have not been merged.

Delete a remote branch:

```bash
git push origin --delete feature
```

---

# Stash

Temporarily store uncommitted work:

```bash
git stash
```

Then restore it:

```bash
git stash pop
```

List stashes:

```bash
git stash list
```

Create a named stash:

```bash
git stash push -m "WIP navigation changes"
```

Apply without deleting the stash:

```bash
git stash apply
```

`pop` applies and removes it.

`apply` applies and keeps it.

## Include Untracked Files

By default, untracked files are not included.

Use:

```bash
git stash -u
```

when necessary.

---

# Cherry-Pick

Apply a specific commit from somewhere else onto the current branch:

```bash
git cherry-pick <commit>
```

Example:

```bash
git cherry-pick a83d927
```

Useful when:

> "I want that particular commit, but I do not want to merge the entire branch."

Cherry-picking creates a new commit on the current branch containing those changes.

---

# Undo the Last Local Commit but Keep the Changes

If you committed too early:

```bash
git reset --soft HEAD~1
```

The commit disappears, but its changes remain staged.

To keep the changes but unstage them:

```bash
git reset HEAD~1
```

Be considerably more cautious with:

```bash
git reset --hard
```

because it can destroy local changes.

---

# Revert a Commit Safely

For commits already shared/pushed, prefer:

```bash
git revert <commit>
```

Rather than deleting history, Git creates a new commit that reverses the earlier commit.

Example history:

```text
A---B---C
```

Reverting `C` creates:

```text
A---B---C---D
```

where `D` reverses the changes introduced by `C`.

This is generally safer for shared branches than rewriting history.

---

# Reset vs Revert

Mental model:

```text
reset
    → move/change local history

revert
    → create a new commit that undoes old work
```

Use `revert` for history that has already been shared.

Use `reset` primarily for local history you control.

---

# Recovering "Lost" Work With Reflog

Git often retains references to commits even after they appear to have disappeared.

Use:

```bash
git reflog
```

Example:

```text
a72bc91 HEAD@{0}: reset: moving to HEAD~1
4bf193a HEAD@{1}: commit: Implement navigation
```

If `4bf193a` was accidentally removed from normal history, it can often be recovered.

For example:

```bash
git switch -c recovery 4bf193a
```

`reflog` is one of the most useful recovery tools in Git.

Before assuming work is permanently gone:

```bash
git reflog
```

---

# Clean Untracked Files

Preview what Git would delete:

```bash
git clean -n
```

Include directories in the preview:

```bash
git clean -nd
```

Actually remove untracked files:

```bash
git clean -f
```

Remove untracked directories too:

```bash
git clean -fd
```

## Important

Always preview first:

```bash
git clean -nd
```

Then, if the result is correct:

```bash
git clean -fd
```

Unlike `git rm`, `git clean` targets **untracked** content.

Mental model:

```text
git rm      → tracked files
git clean   → untracked files
```

---

# Tags

List tags:

```bash
git tag
```

Create an annotated tag:

```bash
git tag -a v1.0.0 -m "Version 1.0.0"
```

Push the tag:

```bash
git push origin v1.0.0
```

Push all local tags:

```bash
git push --tags
```

---

# Inspect a File's History

Show commits affecting a file:

```bash
git log -- path/to/file
```

Show the patches as well:

```bash
git log -p -- path/to/file
```

See who last changed individual lines:

```bash
git blame path/to/file
```

Despite the wonderfully accusatory name, `git blame` is often useful for finding the commit that introduced a line so the surrounding context can be inspected.

---

# Compare Branches

Show commits in `feature` that are not in `main`:

```bash
git log main..feature --oneline
```

Show code differences:

```bash
git diff main..feature
```

Compare against the common ancestor of the branches:

```bash
git diff main...feature
```

The two-dot and three-dot forms have different semantics, so do not treat them as interchangeable.

For ordinary feature-review purposes, the three-dot form is often useful because it asks roughly:

> "What has this feature changed since it diverged from main?"

---

# Useful Branch Workflow

A straightforward feature workflow:

```bash
git switch main
git pull

git switch -c feature-name
```

Work normally:

```bash
git status
git add .
git commit -m "Implement feature"
```

If `main` changes significantly before the feature is finished:

```bash
git fetch origin
git rebase origin/main
```

Then push:

```bash
git push -u origin feature-name
```

After integration:

```bash
git switch main
git pull
git branch -d feature-name
```

---

# Replacing an Existing Project While Preserving Git History

Useful when completely rebuilding a repository.

Create a branch for the replacement:

```bash
git switch -c redesign
```

Remove all tracked project files:

```bash
git rm -rf .
```

Inspect remaining files:

```bash
git status
ls -la
```

Remember that ignored and untracked files remain.

Build the replacement project inside the existing repository.

Then:

```bash
git add .
git status
git commit -m "Replace legacy site with redesigned application"
```

This preserves the repository and its history while allowing the implementation to be completely replaced.

---

# Useful Safety Habits

Before potentially destructive operations:

```bash
git status
```

If unsure what changed:

```bash
git diff
git diff --staged
```

Before deleting untracked files:

```bash
git clean -nd
```

Before assuming a commit is lost:

```bash
git reflog
```

Before force pushing, stop and understand **why** a normal push is being rejected.

---

# Force Pushes

Avoid:

```bash
git push --force
```

when possible.

If rewriting your own remote feature branch after a rebase requires a force push, prefer:

```bash
git push --force-with-lease
```

`--force-with-lease` refuses to overwrite the remote branch if it has changed in a way your local repository does not expect.

It is still a history-rewriting operation, but it provides an important additional safety check.

---

# Useful Aliases

Aliases are optional. Avoid creating so many that Git commands become incomprehensible on another machine.

A few reasonable examples:

```bash
git config --global alias.st status
git config --global alias.br branch
git config --global alias.sw switch
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Then:

```bash
git st
git br
git sw main
git lg
```

However, there is value in knowing and using the actual Git commands. Only alias commands that genuinely improve the workflow.

---

# Quick Reference

## Status

```bash
git status
git status --short
```

## Changes

```bash
git diff
git diff --staged
```

## Stage

```bash
git add file
git add .
git add -p
```

## Commit

```bash
git commit -m "Message"
git commit --amend --no-edit
```

## Branch

```bash
git branch
git branch -a
git switch branch
git switch -c new-branch
```

## Update

```bash
git fetch
git pull
```

## Push

```bash
git push
git push -u origin branch
```

## Restore

```bash
git restore file
git restore --staged file
```

## Remove Tracked Files

```bash
git rm file
git rm -rf .
git rm --cached file
```

## Remove Untracked Files

```bash
git clean -nd
git clean -fd
```

## Merge

```bash
git merge branch
git merge --abort
```

## Rebase

```bash
git rebase main
git rebase --continue
git rebase --abort
```

## Stash

```bash
git stash
git stash -u
git stash list
git stash pop
```

## Undo / Recovery

```bash
git revert <commit>
git reset --soft HEAD~1
git reflog
```

## History

```bash
git log
git log --oneline
git log --oneline --graph --decorate --all
```

---

# Commands Worth Remembering Conceptually

Rather than memorizing every option, remember what these commands **mean**:

```text
status      Where am I and what's changed?
diff        What exactly changed?
add         Put this change in the next commit.
commit      Record this snapshot.
switch      Take me to another branch.
restore     Restore file state.
fetch       Tell me what changed remotely.
pull        Fetch and integrate remote work.
push        Publish my commits.
merge       Combine histories.
rebase      Replay my work on a different base.
stash       Put this unfinished work aside temporarily.
cherry-pick Give me that specific commit.
revert      Make a new commit that undoes an old one.
reset       Move local history/state.
rm          Remove tracked content.
clean       Remove untracked content.
reflog      Show me where HEAD has been.
```

When Git gets confusing, return to:

```bash
git status
```

Then determine which state you actually want to change.