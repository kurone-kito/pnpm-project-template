# Guidelines for AI Agents

This project is a generic Node.js project template using pnpm.
It supports both monorepo and non-monorepo configurations,
providing a common foundation that can be specialized for either
structure. It is derived from the language-independent
[template](https://github.com/kurone-kito/template) repository.

This file is the canonical, tool-neutral instruction source for AI coding
agents in this repository, following the [AGENTS.md](https://agents.md)
convention that most agent tools discover automatically at the repository
root. `CLAUDE.md` and `GEMINI.md` are thin adapters that import this file
for the tools that do not read it by default, and
[.github/copilot-instructions.md](.github/copilot-instructions.md)
carries only the small amount of guidance specific to GitHub Copilot's own
UI. See [docs/ai-strategy.md](docs/ai-strategy.md) for the reasoning behind
this layout.

When contributing to this repository using AI agents, adhere to the
following guidelines to ensure high-quality contributions that align
with the project's standards and practices:

## Conversation

- The conversational language should match the user's language.
  For example, if the user speaks in Japanese, respond in Japanese.
- However, comments and documentation should be written in English unless
  there is a clear context otherwise.
- Continue autonomously for low-risk work, but pause and ask a concise
  question when uncertainty or hidden risk makes the next step unsafe. When
  that pause is needed, provide one or more recommended response options.

## Branch strategy

This project follows
[GitHub Flow](https://docs.github.com/en/get-started/using-git/github-flow):
`main` is the only long-lived branch and every change reaches `main`
through a pull request.

### Rules

- **Never push directly to `main`** — all changes must go through a pull
  request. GitHub enforces this via a repository ruleset targeting
  `main` (`non_fast_forward`, `deletion`, and `pull_request` rules) —
  check `gh api repos/<owner>/<repo>/rulesets`, not the legacy
  `branches/main/protection` endpoint, which does not cover rulesets
  and returns 404 even when one is active.
- **Rebase onto `main`** — when a feature branch needs the latest `main`,
  always rebase. Fetch first so the local `main` is not stale,
  e.g. `git fetch origin && git rebase origin/main`
  (or `git pull --rebase origin main`). Do not create merge commits inside
  feature branches.
- **Rebase between feature branches** — if one feature branch needs
  changes from another, use rebase, not merge.
- **Merge commits at PR boundary** — pull requests into `main` are merged
  with a merge commit (squash-merge and rebase-merge are disabled in the
  repository settings).
- **fixup + autosquash for in-branch fixes** — when a later commit in a
  feature branch fixes an earlier one, prefer `git commit --fixup=<sha>`
  followed by `git rebase -i --autosquash main` (name the base branch
  explicitly; without it, a branch with no tracking information fails
  outright, and one already in sync with its upstream silently no-ops
  instead of folding the fixup commit) to fold the fix into its target.
- **Avoid giant commits** — if squashing would produce an unreasonably
  large commit, keep the fix commit separate or re-split the history so
  each commit remains reviewable.

## Boundaries

### Always do

- Run `pnpm run lint:fix` after every change, then verify with
  `pnpm run lint`
- Follow Conventional Commits for all commits
- Use LF line endings, 2-space indentation, and a final newline
- Keep commits atomic — one logical change per commit
- Write comments and documentation in English

### Ask first

- Adding or removing dependencies
- Changing the project architecture or directory structure
- Modifying CI/CD workflows (`.github/workflows/`)
- Altering shared configuration packages (`@kurone-kito/*-config`)
- Making changes that affect all workspace packages

### Never do

- Commit secrets, credentials, API keys, or tokens into source code
- Modify community documents (`CODE_OF_CONDUCT*`, `CONTRIBUTING*`)
  without explicit approval
- Disable or bypass linter rules without justification
- Accept AI-generated code without reviewing it for correctness
  and security
- Introduce breaking changes without a `BREAKING CHANGE` footer

## Commit rules

This project follows
[Conventional Commits](https://www.conventionalcommits.org/).
A `.gitmessage` template is available at the repository root for
guidance when writing commit messages. Git does not use it automatically,
so contributors who want the template prefilled in their editor should
opt in once per clone:

```sh
git config commit.template .gitmessage
```

### Format

```txt
<type>[optional scope]: <user-facing description>

<body: address purpose, context, and what changed>

[optional footer(s)]
```

### Subject line

- Use the format: `<type>[optional scope]: <description>`
- Write from the **user's perspective** — briefly state what this
  commit solves or improves for the end user or developer
- Write in **lowercase**, imperative mood (e.g., "add", not "added")
- Keep the subject line under **72 characters**
- Do **not** end with a period

### Types

Common types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`,
`chore`, `ci`, `build`, `perf`

### Scopes

- Optional, in parentheses: `feat(ci):`, `fix(lint):`, `docs(readme):`
- Keep scopes **lowercase**, short, and consistent
- Use the directory or component name that best describes the area

### Body (line 3+)

The body should address three aspects:

- **Why** — the purpose or motivation behind the change
- **Context** — what was needed, the situation or constraint
- **What changed** — the concrete action taken

Prefer the **why → context → change** order when practical.
Write these as **natural prose** — weave the aspects into
coherent sentences rather than using labeled sections. Labeled
sections (`Why:` / `Context:` / `Change:`) are acceptable only
when explicit paragraph separation improves clarity.

Omit any aspect whose information **cannot be reliably inferred**.
If the subject line is self-explanatory, the body may be omitted
entirely. **Breaking changes must always include a body.**

Wrap body lines at **72 characters**.

### Breaking changes

- Append `!` after the type/scope: `feat!: remove deprecated endpoint`
- Add a `BREAKING CHANGE:` trailer in the footer with a detailed
  explanation of what breaks and migration steps

### Footers / trailers

- `Closes #<issue>` / `Refs #<issue>` — link to issues
- `Co-authored-by: Name <email>` — credit co-authors
- `BREAKING CHANGE: <description>` — detail the breaking change

### Atomic commits

Keep each commit as **small and focused** as possible:

- **One logical change per commit** — if the subject line needs "and",
  consider splitting
- **Separate refactoring** from behavior changes
- **Separate formatting/style** changes from logic changes
- **Separate dependency updates** from code changes
- When in doubt, prefer smaller commits that are easy to review,
  revert, and bisect

### Examples

#### Good — single-line (trivial change)

```txt
fix: correct typo in feature request template
```

#### Good — prose body

```txt
feat(ci): add concurrency settings to lint workflow

Parallel lint runs on the same branch waste resources and
cause race conditions in status checks. GitHub Actions
supports concurrency groups that automatically cancel
redundant runs, so add a concurrency group keyed on branch
name with cancel-in-progress enabled.

Refs #42
```

#### Good — breaking change

```txt
feat!: require node 20 as minimum version

Node 18 reached end-of-life in April 2025 and no longer
receives security updates, while the project now standardizes
on the active Node 20 LTS baseline. All production
environments have already been upgraded to node 20+, so
update the engines field and CI matrix to require node >= 20.

BREAKING CHANGE: drop support for node 16 and 18. Users
must upgrade to node 20 or later.
Closes #108
```

#### Bad — vague, developer-centric

```txt
fix: update code
```

#### Bad — too large / non-atomic

```txt
feat: add auth system and refactor database layer and update docs
```

## Coding standards

- **Indentation**: 2 spaces (enforced by `.editorconfig`)
- **Line endings**: LF only (enforced by `.editorconfig` and
  `.gitattributes`)
- **Trailing whitespace**: trimmed (except in Markdown)
- **Final newline**: always present
- **File naming**: lowercase with hyphens (e.g., `feature-request.yml`)
  unless constrained by a platform convention (e.g., `CONTRIBUTING.md`)

## Development

### Install the dependencies

```sh
corepack enable
pnpm install
```

### Linting

```sh
pnpm run lint
pnpm run lint:fix # Lint and auto-fix
```

### Testing

```sh
pnpm run test
```

Currently, the command works as an alias for the `pnpm run lint` command.
Set up your own testing framework and replace this script as needed.

### Cleaning

```sh
pnpm run clean
```

## Testing strategy

Currently, `pnpm run test` is an alias for `pnpm run lint`. When
setting up a derived project, replace this with a real test runner
(e.g., Vitest, Jest) and define:

- **Test location**: co-locate test files next to source or in a
  dedicated `__tests__/` directory
- **Coverage targets**: aim for meaningful coverage; avoid
  coverage-only metrics
- **Test naming**: use descriptive names that explain the expected
  behavior (e.g., `it('returns 404 when user not found')`)
- **CI integration**: ensure tests run in the CI pipeline
  (`.github/workflows/`)

## Monorepo guidance

This template supports pnpm workspaces. When working in a monorepo
configuration:

- **Scoped commands** — prefer `pnpm --filter <package>` over
  running commands at the root to save time and reduce noise
- **Nested AGENTS.md** — consider adding an `AGENTS.md` inside each
  workspace package with package-specific instructions; the nearest
  file in the directory tree takes precedence
- **Package naming** — check the `name` field in each package's
  `package.json` to confirm the correct package name
- **Dependency boundaries** — respect workspace package boundaries;
  avoid circular dependencies between packages

## Guardrails

- **Do not** modify community documents (CODE_OF_CONDUCT, CONTRIBUTING)
  without explicit approval

## Security

These rules follow the
[OpenSSF Security-Focused Guide for AI Code Assistant Instructions](https://best.openssf.org/Security-Focused-Guide-for-AI-Code-Assistant-Instructions.html):

- **No secrets in code** — store credentials in environment variables
  or a secrets manager; never hard-code them
- **Treat AI output as untrusted** — review all generated code for
  correctness, security vulnerabilities, and adherence to project
  standards before committing
- **Validate inputs** — ensure all external data is validated and
  sanitized before use
- **Verify dependencies** — confirm that any recommended packages are
  reputable, actively maintained, and free of known vulnerabilities
- **Recursive review** — when generating security-sensitive code, ask
  the AI to review its own output and suggest improvements before
  accepting

## Onboarding

This project template is a Node.js / pnpm specific derivative of the
generic [template](https://github.com/kurone-kito/template).
If you plan to customize this template for a different framework or
toolchain, **submit a proposal to update the documentation first**.

### Derived-repository detection

When an AI agent starts a session, it should determine whether this
repository is the **base template** or a **derived project**:

1. **Check the repository name** — inspect the git remote URL
   (e.g., `git remote get-url origin`), the working-directory name,
   or any GitHub API context available to the agent. If the
   repository name is exactly `pnpm-project-template`, treat it as
   the base template. Any other name indicates a derived project.
2. **Check for generic content** — look for the sentinel phrase
   `generic Node.js project template using pnpm` in this file or
   in the repository's AI instruction files. Its presence means the
   guidelines have **not yet been customized**.

If both conditions are met — the repository is derived **and** the
guidelines are still generic — the agent should **proactively
propose an onboarding workflow** before proceeding with the user's
request. The proposal should be conversational, brief, and
non-blocking (the user may decline and continue normally).

### Onboarding proposal

When proposing onboarding, suggest customizing the following areas
in a single plan:

1. **Project description** — update `README.md` and the opening
   lines of AI instruction files to reflect the project's purpose
2. **Framework / toolchain** — identify the primary framework;
   add relevant build tooling and type definitions
3. **Dependency management** — configure workspace structure and
   lock-file conventions as needed
4. **Testing strategy** — define the test runner, coverage targets,
   and test-file conventions
5. **CI/CD workflows** — adjust `.github/workflows/` to match the
   project's build, test, and deploy pipeline
6. **AI guideline specialization** — rewrite this file with
   project-specific rules, coding patterns, and architecture notes.
   The adapters (`CLAUDE.md`, `GEMINI.md`,
   `.github/copilot-instructions.md`) rarely need changes, since they
   import or reference this file rather than duplicating it.
7. **README rewrite** — replace the template README with
   project-specific content (badges, installation, usage, etc.)
8. **License review** — confirm or replace the MIT license if the
   project requires a different one

Present these items as a checklist proposal and let the user select
which items to tackle and in what order.
