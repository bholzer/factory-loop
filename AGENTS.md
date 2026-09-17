# harness-blueprint — agent guide

This repository builds a harness-portable blueprint for agentic development
flows: a template payload of repository artifacts (`template/`) plus portable
skills (`skills/`) that let an in-project agent bootstrap, operate, and
maintain its own harness. Nothing here compiles; every artifact is markdown
operated on with shell and git. The one thing to understand before editing:
`template/` is the payload a target project receives and knows nothing about
this repository, while the root artifacts you are reading are that same
payload filled in for this project — so a generic fix belongs in both halves,
and a reference from `template/` outward is a bug.

## Map

- `GOALS.md` — purpose, outcomes, success conditions, scope, non-goals,
  constraints, known unknowns. Read first.
- `ARCHITECTURE.md` — components, layer map, allowed dependency directions.
- `docs/PRINCIPLES.md` — the few rules that override local convenience.
- `docs/MATURITY.md` — the autonomy ladder and what gates each rung.
- `docs/DEBT.md` — known debt and deferred work, each with enough context to
  pick up cold.
- `docs/decisions/` — one short file per durable decision; format in
  `docs/decisions/DECISION_FORMAT.md`.
- `docs/specs/` — living descriptions of how the system currently behaves.
- `docs/capabilities/` — spec cards for mechanical enforcers, with status;
  format in `docs/capabilities/CARD_FORMAT.md`.
- `plans/PLANS.md` — the plan convention. All multi-file or multi-session
  work follows it.
- `plans/active/` — work in flight. `plans/completed/` — done.
- `skills/<name>/SKILL.md` — the portable procedures an agent invokes; each
  one references the artifacts above rather than restating them.
- `template/` — the payload copied into a target project and filled in there.

## Commands

There is no build, test, or lint toolchain: the artifacts are markdown, so
there is nothing to compile and no suite to run. Two checks are cheap enough
to run after any edit.

- Convention identity (silence is a pass):
  `cmp template/plans/PLANS.md plans/PLANS.md`
- Live artifacts carry no leftover authoring scaffolding (the bracketed
  letters keep the pattern from matching this file):
  `grep -rn '{{FIL[L]\|GUIDANC[E]' AGENTS.md GOALS.md ARCHITECTURE.md
  docs/PRINCIPLES.md docs/MATURITY.md docs/DEBT.md docs/specs/index.md
  docs/capabilities/index.md` — silence is a pass; any output names a file
  still holding template markers.

The wider template ↔ live structural walk is hand-run and unmechanized. What a
mechanism must do is specified in
`docs/capabilities/template-live-drift.md`; the hand procedure and its blind
spots are in `docs/DEBT.md` under `D1`.

## Working rules

- Multi-file or multi-session work requires an ExecPlan per `plans/PLANS.md`.
- Execute one milestone per session. Update the plan's living sections
  (Progress, Decision Log, Surprises) with observed evidence. Stop.
- Never report a planned command as passing evidence. Run it, record what you
  observed.
- Decisions with lasting effect go in the plan's Decision Log; durable ones
  graduate to `docs/decisions/`.
- Nothing material lives outside the repo. If it was decided, it is written
  here.
- This file is a map, not an encyclopedia. Keep it under ~100 lines; content
  that wants to grow moves into `docs/` and leaves a one-line map entry.
- Respect the layer map in `ARCHITECTURE.md`: nothing under `template/` may
  name a path outside `template/`, and a generic improvement to a live root
  artifact is unfinished until `template/` carries it too.
