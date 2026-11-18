# Tasks and scripts

How a repository pins its tools and names its tasks, so every project is set up, checked and deployed the same way,
locally and in CI. [mise](https://mise.jdx.dev) holds both the tools and the tasks.

## Tools

- Every dev tool the repository needs is pinned to an exact version in `[tools]` of `mise.toml`: runtimes, package
  managers and command-line tools alike, also in a repository without a build. CI reads the same file, so local and
  CI always run the same versions.
- `[env]` puts the project's locally installed binaries on the path, so a task calls them directly instead of through
  the package manager. For a JavaScript project:

  ```toml
  [env]
  _.path = ["{{config_root}}/node_modules/.bin"]
  ```

Which tools and versions a project uses is not part of this standard.

## Tasks

- Tasks live only in mise, not in the package manager's scripts (no npm scripts, also not in workspaces).
- A task name starts with a verb from this list: `setup`, `dev`, `preview`, `build`, `gen`, `fetch`, `check:*`,
  `fix:*`, `test`, `ci`, `deploy` and `prod:*`. A subject follows after a colon where needed
  (`setup:browsers`, `gen:song-index`, `test:e2e`).
- Work against an external service is `prod:*`, also when it is not the project's own production (a form service,
  a mailing list). `deploy` and `prod:*` tasks ask with mise `confirm`, unless the tool itself already does a dry run
  by default and asks to type YES; then `confirm` adds nothing.
- Every task has a `description`. A task that only reads says so; a task that rewrites files says that too.
- Every flag gets a `usage` spec with `help`, and where possible `default` and `choices`.
- A task with more than a few lines of logic becomes a file task in `mise-tasks/`.

### Pitfalls

- **A path argument** needs `#MISE dir="{{cwd}}"` and `realpath`. Otherwise the path is relative to the repository
  root instead of where the command was typed.
- **macOS has bash 3.2.** An empty array under `set -u` fails, so write `${a[@]+"${a[@]}"}`. `realpath` there has
  no `-m`.

## Checks

- Every repository has `check:lint`, `check:format` and `check:types`. Bare `check` depends on every `check:*`,
  and every `check:x` that a tool can repair has a `fix:x`; bare `fix` depends on every `fix:*`.
- Which tool does the checking is up to the repository. A repository without JavaScript gets what fits, such as
  `check:syntax`.
- A deliberate exception to a lint rule gets a disable comment with the reason.
- When a formatter is introduced, its big formatting change goes in a commit of its own. Check that the build
  output does not change.

## CI

- Every action is pinned to a commit SHA with the version as a comment. mise is installed with `jdx/mise-action`.
- The steps are `mise run setup`, `mise run setup:browsers` when browser tests run, and then `mise run ci`.
- A pull request runs `mise run ci`. `main` runs `mise run --yes deploy`, which depends on `ci` or `build`. A
  repository without a deploy runs `ci` on both.
- `ci` is everything except a deploy. A few slow suites may be a job of their own.

## Documentation

- This convention lives only here. A project's README mentions at most `mise run setup` and `mise tasks`, and
  perhaps `mise run test`.
- The README, docs, code comments, error messages and tests use the mise task names. ADRs and other historical
  documents stay as they were written.
