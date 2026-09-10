# AI tooling strategy

This repository's day-to-day AI work now centers on Claude Code, with
other AGENTS.md-aware harnesses (Codex CLI, GitHub Copilot, Gemini CLI,
and similar tools) available as fallbacks when Claude Code is
unavailable. The AI-instruction layout follows that harness mix.

## Canonical guidance

- [AGENTS.md](../AGENTS.md) is the canonical, fully detailed AI guide.
  It follows the [AGENTS.md](https://agents.md) convention —
  a vendor-neutral standard now stewarded by the Linux Foundation's Agentic
  AI Foundation — that most agent tools discover automatically at the
  repository root, including OpenAI Codex, GitHub Copilot (CLI, coding
  agent, and Chat), xAI Grok Build, and Google Antigravity. Keep new
  guidance here first.
- [CLAUDE.md](../CLAUDE.md) and [GEMINI.md](../GEMINI.md) are thin adapters
  for the two tools that do not read `AGENTS.md` by default (Claude Code,
  and Gemini CLI unless a user has opted into `AGENTS.md` in their own
  settings). Each imports `AGENTS.md` via a standalone `@AGENTS.md`
  directive so its content loads automatically; they should stay a few
  lines and rarely need edits.
- [.github/copilot-instructions.md](../.github/copilot-instructions.md)
  is a thin GitHub Copilot adapter. Copilot already auto-discovers
  `AGENTS.md` directly, so this file only carries the one genuinely
  Copilot-specific note: mapping the shared "pause and ask when risky"
  guidance onto Copilot's own Agent mode / Plan mode terminology.

## Change policy

- `AGENTS.md` is the source of truth. Adapters exist only to get the
  content in front of tools that would otherwise miss it — do not
  duplicate guidance into them.
- When a rule needs tool-specific vocabulary (like Copilot's Agent mode /
  Plan mode), keep the neutral wording in `AGENTS.md` and put the
  vocabulary mapping in that tool's own adapter.

## Onboarding detection

When the repository name is not `pnpm-project-template` and the AI
instruction files still contain the generic sentinel phrase, AI agents
should proactively propose a customization workflow. This keeps the
template's "vibe-coding ready" promise alive in derived projects by
guiding users through documentation, tooling, and guideline specialization
immediately after they create a new repository from the template. The
full onboarding checklist is maintained in `AGENTS.md` § Onboarding.

## Maintenance notes

- Treat this file as a human-facing strategy note, not as the primary
  instruction file for any agent.
- When updating AI guidance, review `AGENTS.md` first, then `CLAUDE.md`,
  `GEMINI.md`, `.github/copilot-instructions.md`, and `README.md` for
  anything that references it.

## History

This repository previously ran a Copilot-first layout, where
`.github/copilot-instructions.md` was canonical and `AGENTS.md`,
`CLAUDE.md`, and `GEMINI.md` were near-duplicate compatibility entry
points, with further consolidation deferred until benchmarks justified it.
That policy fit a Copilot-centric workflow, but as Claude Code became the
primary tool and more fallback harnesses were added, the duplication cost
grew faster than any benchmark was going to resolve, and every new adapter
file meant another copy to keep in sync. The current layout replaces that
policy outright, at the project owner's request, in favor of the
single-source-plus-adapters structure described above — matching the same
transition already made in the upstream
[template](https://github.com/kurone-kito/template) repository.
