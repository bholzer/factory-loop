# harness-blueprint — agent guide

This repository builds a harness-portable blueprint for agentic development
flows: a template payload (`template/`) plus portable skills (`skills/`).
This repo is client #1 of its own payload — root artifacts here are the live
instantiation of what `template/` ships.

## Map

- `GOALS.md` — what this project is and is not. Read first.
- `plans/PLANS.md` — the ExecPlan convention. All multi-session work follows it.
- `plans/active/` — work in flight. `plans/completed/` — done.

Forthcoming, per the active plan: `template/`, `skills/`, `docs/`,
`ARCHITECTURE.md`. This map grows as they land.

## Working rules

- Multi-file or multi-session work requires an ExecPlan per `plans/PLANS.md`.
- Execute one milestone per session. Update the plan's living sections
  (Progress, Decision Log, Surprises) with observed evidence. Stop.
- Never report a planned command as passing evidence. Run it, record what you
  observed.
- Decisions with lasting effect go in the plan's Decision Log (once
  `docs/decisions/` exists, durable ones graduate there).
- Nothing material lives outside the repo. If it was decided, it is written
  here.
