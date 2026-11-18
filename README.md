# bvleur tech projects

Bram's standards for his tech projects, and the skills that let Claude follow them.

- `standards/` holds the standards themselves, one document per topic. Projects link to them, for example
  "Tasks follow [tasks and scripts](https://github.com/bvleur/bvleur-tech-projects/blob/main/standards/tasks-and-scripts.md)".
- `ai-skills/` holds the skills for Claude. A skill about a standard only points to its document in `standards/`, so
  each rule lives in one place. A skill that is only about working with Claude (`working-agreements`) holds its
  rules itself.

## Using it

The repository is a Claude plugin marketplace with one plugin, `bvleur-tech-projects`.

- **Claude Code on your machine:** `claude plugin marketplace add bvleur/bvleur-tech-projects`, then
  `claude plugin install bvleur-tech-projects@bvleur-tech-projects`.
- **claude.ai:** add this repository as a marketplace and turn the plugin on.
- **Claude Code in the cloud:** cloud sessions do not load plugins. A project gets a SessionStart hook that fetches
  the standards instead (planned, in the `new-project` skill).
