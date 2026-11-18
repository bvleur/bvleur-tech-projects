# Tasks and scripts

How a repository pins its tools and names its tasks, so every project is set up, checked and deployed the same way,
locally and in CI. [mise](https://mise.jdx.dev) holds both the tools and the tasks.

## Names say what a task changes

A task name is predictable: its verb says what the task may change, its side effect. Whoever runs a task, a developer
or an agent, knows from the verb alone whether it only reads, rewrites code, makes files, or changes something
outside the machine, without reading the task. So tasks are split along their side effects: one task never mixes a
read-only check with a rewrite, or a local build with a change on a server.

From no side effect to the largest:

| Verb | Side effect | Example |
|---|---|---|
| `check:*` | Reads the repository only. Changes no file; safe to run at any moment. | `check:lint`, `check:types` |
| `test`, `test:*` | Runs tests. Changes no source file; writes at most reports and caches that git ignores. | `test:e2e` |
| `dev`, `preview` | Runs a local server. Changes no source file. | `dev`, `preview` |
| `setup`, `setup:*` | Installs what the repository needs on this machine (dependencies, test browsers), ignored by git. | `setup:browsers` |
| `build`, `build:*` | Writes build output, ignored by git. | `build:web` |
| `gen:*` | Makes or overwrites generated files in the working copy, which may be committed. | `gen:song-index` |
| `fetch:*` | Downloads from the network into the working copy. Changes nothing remote. | `fetch:samples` |
| `fix`, `fix:*` | Rewrites source files in place: the repairing counterpart of a `check:*`. | `fix:format` |
| `ci` | Everything CI checks, tests and builds. Never changes anything remote. | `ci` |
| `deploy` | Changes the project's own production. Asks for confirmation. | `deploy` |
| `prod:*` | Changes or acts on an external service, also one that is not the project's own production. Asks for confirmation. | `prod:<action>` |

The verbs also say what is safe to run without asking: `check`, `test` and `ci` always are; `deploy` and `prod:*`
never are. A task whose side effect does not fit its verb is renamed or split.

## Tools

- Every dev tool the repository needs is pinned to an exact version in `[tools]` of `mise.toml`: runtimes, package
  managers and command-line tools alike, also in a repository without a build. CI reads the same file, so local and
  CI always run the same versions.
- `[env]` puts the project's locally installed command-line tools on the path, so a task calls them directly instead
  of through a package manager.

Which tools and versions a project uses is not part of this standard.

## Tasks

- Tasks live only in mise, never in a package manager's scripts.
- A task name is a verb from the table above, with a subject after a colon where needed (`verb:subject`).
- **Every part can run on its own, and the bare verb runs all parts.** `check` depends on every `check:*`, `fix` on
  every `fix:*`, and `build` on every `build:*`: `build` makes all artefacts, `build:web`, `build:desktop` or
  `build:ios` makes one. A new part is added to its umbrella in the same change.
- `deploy` and `prod:*` tasks ask with mise `confirm`, unless the tool itself already does a dry run by default and
  asks to type YES; then `confirm` adds nothing.
- Every task has a `description` that names its side effect where the verb leaves room ("read-only",
  "rewrites files", "needs a build").
- Every flag gets a `usage` spec with `help`, and where possible `default` and `choices`.
- A task with more than a few lines of logic becomes a file task in `mise-tasks/`.

### Pitfalls

- **A path argument** needs `#MISE dir="{{cwd}}"` and `realpath`. Otherwise the path is relative to the repository
  root instead of where the command was typed.
- **A file task in bash runs on bash 3.2 on macOS.** An empty array under `set -u` fails, so write
  `${a[@]+"${a[@]}"}`. `realpath` there has no `-m`.

## Checks

- Every repository has `check:lint` and `check:format`, and `check:types` where the language has types. A
  repository adds what else fits, such as `check:syntax` or `check:links`.
- Every `check:x` that a tool can repair has a `fix:x`.
- A deliberate exception to a lint rule gets a disable comment with the reason.
- When a formatter is introduced, its big formatting change goes in a commit of its own. Check that the build
  output does not change.

## CI

- Every third-party step in CI is pinned to an exact version: a commit SHA, with the version as a comment.
- The steps are `mise run setup`, any other `setup:*` the suites need, and then `mise run ci`.
- A pull request runs `mise run ci`. `main` runs `mise run --yes deploy`, which depends on `ci` or `build`. A
  repository without a deploy runs `ci` on both.
- `ci` is everything except a deploy. A few slow suites may be a job of their own.

## Documentation

- This convention lives only here. A project's README mentions at most `mise run setup` and `mise tasks`, and
  perhaps `mise run test`.
- The README, docs, code comments, error messages and tests use the mise task names. ADRs and other historical
  documents stay as they were written.
