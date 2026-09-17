# Goals

## Purpose

Build a thin, harness-portable blueprint for agentic development flows: a
template payload of repository artifacts plus a small set of portable skills
that let an in-project agent bootstrap, operate, and maintain its own harness.

The blueprint encodes five subsystems: a knowledge layer (repo as the only
system of record), a constraint layer (mechanical invariants, specced not
implemented), a feedback layer (agent-verifiable work), a process layer
(self-contained living plans with evidence discipline), and an entropy layer
(day-one garbage collection of drift and debt).

## Outcomes

A project bootstrapped from this blueprint has:

- A table-of-contents `AGENTS.md` (~100 lines max) pointing into a structured
  docs tree — never an encyclopedia.
- `GOALS.md`, `ARCHITECTURE.md`, `docs/` (principles, maturity ladder, debt
  tracker, decisions, behavior specs, capability specs).
- A plan convention (`plans/PLANS.md`) producing self-contained, restartable,
  milestone-sized ExecPlans that carry their own evidence.
- Six portable skills: `harness-init`, `plan-author`, `plan-execute`,
  `capability-build`, `doc-garden`, `retro`.
- Capability spec cards that guide an in-project agent to build the actual
  mechanical enforcers in whatever stack the project uses.
- Entropy management from the first commit: debt tracker, golden principles,
  drift-scanning cadence.

## Success conditions

- The artifact layer works unmodified in Claude Code, Codex, and omp.
- All artifacts are markdown + git + shell; nothing assumes a stack or harness.
- A fresh-context agent can execute one milestone from a plan file and the
  worktree alone — no conversation history, no external documents.
- A capability spec card is implementable by an in-project agent with no
  knowledge of this repository.
- This repository is client #1: its root artifacts are a live instantiation of
  its own template payload.

## Scope

- Greenfield-first. The bootstrap flow (orient → map → propose → fill) must
  not preclude existing codebases, but carries no brownfield-specific
  machinery.
- Conventions, skills, and spec cards — not implementations of enforcers.
- The maturity ladder (L0 human-gated → L3 bounded autonomy) is documented
  from day one; only L0/L1 behavior is exercised in v1.
- The payload's `isolated-env` card is not instantiated here: nothing in a
  repository of markdown has a runtime to isolate or a toolchain to pin, so a
  live copy of that card would specify a check that every input passes.

## Non-goals

- No stack-specific enforcer implementations (linters, CI configs, env
  scripts). Spec cards describe invariants and acceptance; target-project
  agents build the mechanisms.
- No multi-agent orchestration machinery. One task per loop; the loop lives
  outside the context window.
- No task DAGs, per-milestone files, or per-phase context-file hierarchies.
  Plans compose; they don't nest.
- No harness-specific tool invocations in skill bodies. Skills are procedures
  over files, shell, and git.
- No background automation in v1. `loop-runner` and recurring GC agents are
  specced and parked at L2 on the maturity ladder.
- No `PROMPTS.md`. Reusable workflow entry points are what skills are.

## Constraints

- The fresh-context loop binds every session here: milestone sizing, the
  one-milestone session, and the evidence bar are stated in `plans/PLANS.md`
  and are that file's to state.
- Every specced mechanical enforcer must emit remediation instructions in its
  failure output — error messages are context injection.
- Anything material decided outside the repo (chat, review, head) must land in
  the repo to exist. What the agent cannot reach in-context does not exist.
- A human gate retires only when a mechanical check subsumes it and has been
  green for N cycles.

## Known unknowns

- Evaluation is paper-only in v1. A live-trial process (bootstrap a toy repo
  in ≥2 harnesses, drive one feature through the loop) is specced as a
  capability card, unbuilt.
- How `boundary-lint` retrofits onto a large existing codebase.
- Whether `capability-build` eventually needs per-stack reference material or
  stays fully generic.
