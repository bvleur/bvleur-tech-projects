---
name: tasks-and-scripts
description: Bram's standard for pinning tools and naming tasks with mise (mise.toml, mise-tasks/, check/fix/build/test/ci/deploy tasks, CI steps). Use when adding, renaming or running a task, editing mise.toml or a CI workflow, or setting up checks in one of Bram's repositories.
---

The standard is `../../standards/tasks-and-scripts.md`, relative to this file. Read it and follow it; this skill
holds no rules of its own beyond the two steps below.

## Make the project point to the standard

A project that follows the standard says so in its own files, so that Claude finds the standard while working in the
project, also where this plugin is not loaded (cloud sessions). When you apply the standard to a project, or change a
task in a project that lacks these references, add them:

1. **`CLAUDE.md`** at the project root (create it when it is missing) gets this line under a heading such as
   "Standards":

   ```markdown
   - Tasks and tools follow the [tasks-and-scripts standard](https://github.com/bvleur/bvleur-tech-projects/blob/main/standards/tasks-and-scripts.md).
     Read it before adding, renaming or changing a task in `mise.toml` or `mise-tasks/`.
   ```

2. **`mise.toml`** starts with a comment that points to the same file, so whoever opens the tasks sees it:

   ```toml
   # Tasks and tools follow https://github.com/bvleur/bvleur-tech-projects/blob/main/standards/tasks-and-scripts.md
   ```

The link goes to the file view on GitHub. The repository is public, so it opens without signing in.

## Where the project differs

Where a repository does not follow the standard yet, say so and propose the change instead of copying the old
pattern. A deliberate deviation is written in the project's `CLAUDE.md`, next to the link, with its reason.
