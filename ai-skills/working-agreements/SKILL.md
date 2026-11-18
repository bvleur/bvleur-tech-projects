---
name: working-agreements
description: How Bram works with Claude in his tech projects - language, who decides what, branches, commits and pull requests, and thinking along about costs and risks. Use at the start of work in any of Bram's repositories, and before naming a branch, committing, opening a pull request, or enabling anything that costs money.
---

# Working agreements

These are the agreements for working with Claude in every one of Bram's tech projects. A project's own docs may add
to them; where a project's docs differ, the project wins and the difference is stated there.

## Language

- Talk to Bram in Dutch.
- Everything in the repository is English: code, comments, commits, branch names, tech docs and decision notes.
- Exception, chosen per project: when the product itself is inherently Dutch (its content and its audience), its
  functional and product docs may be Dutch. The project states this in its own docs. Code, commits, tech docs and
  decision notes stay English.
- Text a user sees goes through the i18n layer, Dutch first, never as a literal string in a component.

## Who decides

- A question the docs leave open goes to Bram as a short proposal that already makes a choice. Nobody decides it
  silently. A choice that changes the goal is Bram's.
- Never invent facts, names, systems or data. What is unknown stays empty, or is written down as an assumption and
  marked as one until Bram confirms it.
- Bram decides what goes to `main`, merges pull requests, deploys to production, and sets billing and spending
  limits. Nobody else changes those.

## Code

- Readable code, no magic, no over-engineering, but structured. Boring over clever: a trick needs a written reason.
- A comment explains why code exists. It explains how only when that is truly complex.

## Branches, commits and pull requests

- Work on a branch and open a pull request. Never push to `main`.
- A branch name is `type/subject`: English, functional, and it says what the change is. Types in use: `feat`, `fix`,
  `docs`, `test`, `ci`, `chore`. Examples: `feat/book-opening`, `fix/preview-entry-sing-along-links`,
  `docs/release-todo`, `ci/deploy-main-and-previews`, `chore/mise-pnpm-node26`.
- Never a generated name such as `claude/project-thread-xxxx`. When a tool assigns one, rename the branch before the
  pull request is opened.
- One topic per branch. Say in the reply which branch to check out.
- A commit is a logical unit, not split too finely. Its message is one plain sentence that says what changed and, when
  it is not obvious, why: "Spare CI minutes on docs-only and unrelated changes, and smoke check the deploy".
- Docs change in the same commit as the code they describe.

## Costs and risks

Bram wants Claude to think along about costs and risks, not only to carry out what is asked.

- **Before enabling anything that costs money, say what it costs.** That covers a paid plan, a product billed per use,
  AI or text-to-speech credits, CI minutes, a scheduled job and a public endpoint. Say what makes the cost grow and
  what caps it, and wait for Bram's go on anything paid.
- **A small fixed monthly cost is fine; a cost that grows without limit with use is not.** Prefer static assets at the
  edge, free tiers, and on-demand over always-on.
- **Design surprise bills out from the start**: a spending limit with alert emails, rate limits at the edge, limits per
  address, and no route that lets a visitor trigger unbounded work.
- **Never pay twice.** Paid output that cannot be made again cheaply (voice-over clips, generated media) is kept, so
  every clone and CI run reuses it.
- **Spare CI minutes**: skip the heavy suites on docs-only changes and run them on demand or on `main`.
- **Name a risk as a risk**: security, privacy, data loss, lock-in, and steps that cannot be undone. A step that cannot
  be undone (a production change, deleting data, renaming a stored class) waits for an explicit go.
