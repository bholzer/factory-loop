# 0008 — Portability is markdown, git, and the intersection of harness conventions

## Decision

Agent instructions live in `AGENTS.md`, which Codex and omp read natively;
Claude Code is served by a root `CLAUDE.md` whose entire content is
`@AGENTS.md`. Skills live at `skills/<name>/SKILL.md` with plain `name` and
`description` frontmatter, and their bodies are procedures over files,
shell, and git only — no harness-specific tool names.

## Rationale

The alternative is a per-harness artifact set, which multiplies every future
edit by the number of harnesses and rots in whichever one is used least. The
intersection of the three harnesses' conventions is markdown files in known
locations, and that intersection turns out to be large enough to carry the
whole blueprint, with one shim file as the only concession.

The accepted cost is verbosity in skill bodies: a procedure that could be a
single tool call in a specific harness is written as a sequence of shell
steps instead. That is the price of the same file working everywhere, and it
also keeps skills readable as procedures a human can follow by hand.

## Date

2026-09-16, v1 blueprint design conversation.

## Status

accepted
