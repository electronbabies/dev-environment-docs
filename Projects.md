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

If it is a development project I maintain, it belongs under `~/code`.

Git repositories may be nested below product directories. The repository audit command discovers them recursively.

---

# Repository Checkpoint

Use `repos` as the central checkpoint for Git state across every repository under `~/code`.

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

## Location

```text
~/code/dotfiles/bin/repos
```

`~/code/dotfiles/bin` is on `PATH`, so the command can be run simply as `repos`.

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
