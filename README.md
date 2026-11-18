# bvleur tech projects

The standards for Bram's tech projects, in one place, in a form that both people and AI coding assistants can work
with.

## Why

Bram's projects share the same ways of working: how tools are pinned, how tasks are named, how docs and decisions are
kept, how code is written. Written down in each project separately, these drift apart. This repository is the single
source:

- **Standardise.** Each standard is written once, here. A project links to it instead of copying it, so a change here
  holds for every project that links to it.
- **Work with it through AI.** Claude (in Claude Code, on claude.ai and in Claude Desktop) gets the standards as
  skills. A skill tells Claude when a standard applies and where to read it, so Claude follows the standard while it
  works in a project, and can bring a project in line with it.

## Structure

| Path | What it holds |
|---|---|
| `standards/` | The standards, one Markdown document per topic. Written for people; this is what projects link to. |
| `ai-skills/` | The skills for Claude, one folder per skill. A skill about a standard only says when it applies and points to its document in `standards/`, so each rule lives in one place. A skill about working with Claude itself (`working-agreements`) holds its rules in the skill. |
| `ai-skills/README.md` | How to add the skills to Claude and how to bring a project up to date. |
| `.claude-plugin/` | The plugin and marketplace manifests that make this repository installable as the Claude plugin `bvleur-tech-projects`. |

## Using it

- **In a project:** link to the standard from the project's `CLAUDE.md`, for example "Tasks and tools follow the
  [tasks-and-scripts standard](https://github.com/bvleur/bvleur-tech-projects/blob/main/standards/tasks-and-scripts.md)".
  The skills add these links themselves when they bring a project in line.
- **In Claude:** add this repository as a plugin marketplace; see [ai-skills/README.md](ai-skills/README.md).

## Changing a standard

Change the document in `standards/` through a pull request. Projects link to `main`, so the new version is what they
point to once it is merged; ask Claude in each project to bring it in line with the change.
