# harness-blueprint — agent guide

This repository is the blueprint's own workshop and its first client. It has
two halves: `template/` is the payload a target project receives and knows
nothing about this repository, and the root artifacts you are reading are that
same payload filled in for this project. `GOALS.md` states what the blueprint
is for; `ARCHITECTURE.md` states which direction the two halves may reference
each other. The one thing to know before editing is that most edits here are
edits to both halves.

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
- `docs/guide/` — the role-and-task guide to this system for humans and
  client projects: what it is, adopting it, operating it, building an
  enforcer. Start at `docs/guide/index.md`.
- `plans/PLANS.md` — the plan convention. All multi-file or multi-session
  work follows it.
- `plans/active/` — work in flight. `plans/completed/` — done.
- `skills/<name>/SKILL.md` — the portable procedures an agent invokes; each
  one references the artifacts above rather than restating them.
- `template/` — the payload copied into a target project and filled in there.
- `CLAUDE.md` — one-line entry-point shim for harnesses that do not read
  `AGENTS.md` natively. Owns nothing; contains only a reference to this file.

## Commands

There is no build, test, or lint toolchain: the artifacts are markdown, so
there is nothing to compile and no suite to run. Three commands exist. One
decides what a machine can decide about this repository; the second builds a
throwaway repository elsewhere and reads the payload the way a target project
receives it; the third drives one milestone session at a time against a plan
in another checkout.

- `./tools/verify` — the cheap verification command. Budget: 5 seconds. It
  runs each executable under `tools/checks/` in a fixed order, streams what
  each one reports, and exits nonzero if any of them finds a violation or
  cannot decide. `docs/capabilities/fast-verify.md` is its card.
- `./tools/blueprint-eval` — the live-trial driver, and not a check: `new
  <label>` copies both halves into a fresh git repository outside this tree
  and prints where, and `check <repo-dir>` reads such a copy and decides
  procedure layout, leftover scaffolding, and whether its paths resolve with
  no access to this tree. `docs/capabilities/blueprint-eval.md` is its card.
- `./tools/loop-runner` — the watched loop driver, and not a check:
  `run <repo-dir> --limit <n> -- <harness invocation>` runs that invocation
  once per milestone, judges each iteration by the commits it left rather
  than by what the session claimed, and halts on the first one that did not
  advance the plan. It refuses this checkout as a target.
  `docs/capabilities/loop-runner.md` is its card.
- `git config core.hooksPath tools/hooks` — once per clone. After it,
  `tools/hooks/pre-commit` refuses any commit that `./tools/verify` rejects.

## Working rules

- Multi-file or multi-session work requires an ExecPlan per `plans/PLANS.md`.
- Execute one milestone per session. Update every living section
  `plans/PLANS.md` requires, with observed evidence. Stop.
- Never report a planned command as passing evidence. Run it, record what you
  observed.
- Decisions with lasting effect go in the plan's Decision Log; durable ones
  graduate to `docs/decisions/`.
- Nothing material lives outside the repo. If it was decided, it is written
  here.
- This file is a map, not an encyclopedia. Keep it under ~100 lines; content
  that wants to grow moves into `docs/` and leaves a one-line map entry.
- Respect the layer map in `ARCHITECTURE.md`. It governs what each half may
  reference and which fixes are unfinished until both halves carry them.
