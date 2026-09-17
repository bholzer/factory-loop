# ExecPlans

An ExecPlan is a self-contained living design document that a stateless agent
can execute to deliver a working, observable change. Any work spanning
multiple files or multiple sessions requires one. Plans live in
`plans/active/` and move to `plans/completed/` when done.

## The core invariant: restartable from the file alone

Treat the executor as a complete novice with only the current worktree and
this one plan file. No conversation history, no prior plans, no external
documents. Every assumption the plan relies on is stated in the plan. Every
term of art is defined in plain language or not used. Required knowledge from
outside sources is embedded in the plan's own words, never linked as a
prerequisite.

If a milestone can only be completed with memory of a previous session, the
plan is broken. Fix the plan, not the session.

## Authoring rules

- **Decisions are made in the plan, not outsourced.** When ambiguity exists,
  the author resolves it in the plan and records why. A plan that asks its
  executor to "choose an appropriate approach" for anything load-bearing is
  incomplete.
- **Prose first.** Narrative sections are sentences, not bullet skeletons —
  reasoning survives in prose and dies in fragments. Checklists belong in
  Progress; short lists elsewhere only when prose would obscure.
- **Over-explain user-visible effects; under-specify incidental
  implementation.** The failure mode to prevent is work that satisfies the
  letter of a milestone but does nothing meaningful. Acceptance guards this:
  behavior, never attributes.
- **Commands are exact and legible.** State the working directory and the
  exact command line; include a short expected output so a novice can tell
  success from failure by comparison.

## Context hygiene

The plan file is durable context; the context window is a disposable cache.

- **Milestone sizing.** A milestone must be completable, verifiable, and its
  living-section updates writable within one fresh-context session — roughly
  one PR-sized change, with acceptance statable as a few observable checks.
  If acceptance cannot be stated tightly, the milestone is too big: split it
  at authoring time.
- **One milestone per invocation.** Each session reads `AGENTS.md`, this
  file's plan, and the files the plan names — nothing else by default.
  Execute one milestone, update living sections, stop. Continuing to the next
  milestone in the same session is prohibited.
- **Overflow means split, not push.** If a milestone will not fit — context
  filling, compaction approaching — split it in place in Progress
  (`completed: X; remaining: Y`), log the decision, and exit. Never work
  through compaction: a compacted session silently violates restartability.
- **Plans compose; they don't nest.** A plan too big for one file is two
  plans, the first naming its successor. No sub-plans, no per-milestone
  files.

## Required sections

Every plan contains these sections. The first four are living sections:
updating them is part of executing any milestone, not optional bookkeeping.

- **Progress** — checkbox list with timestamps. Every stopping point is
  recorded, splitting partially-done items into done/remaining. Always
  reflects actual current state.
- **Decision Log** — every decision made while planning or executing:
  `Decision / Rationale / Date`. It must be unambiguous why the plan changed.
- **Surprises & Discoveries** — unexpected behavior, bugs, insights, with
  concise evidence.
- **Outcomes & Retrospective** — written at completion: what was achieved,
  what remains, lessons. Compared against Purpose.
- **Purpose** — a few sentences: what someone can do after this change that
  they could not before, and how to see it working.
- **Context & Orientation** — current state for a reader who knows nothing.
  Key files by full path. Definitions of non-obvious terms.
- **Interfaces & Dependencies** — prescriptive contracts that later
  milestones or other work rely on: file formats, schemas, signatures, paths,
  statuses. Stated in full — a fresh-context session cannot remember the
  contract an earlier milestone established; the plan is where it lives.
- **Milestones** — narrative, one short block each: scope, what exists at the
  end that didn't before, and observable acceptance. Acceptance is phrased as
  behavior a human can verify, never internal attributes ("running X exits 0
  and prints Y", not "added a helper"). Spike milestones are encouraged when
  requirements carry real unknowns: scoped as prototypes, with explicit
  criteria for promoting or discarding the result.
- **Idempotence & Recovery** — whether steps are safe to re-run; retry or
  rollback paths for any risky step. May be one sentence when everything is
  trivially safe.
- **Validation & Acceptance** — how to exercise the overall result and what
  to observe.

## Evidence rules

- Only observed evidence. Never convert a planned command into passing
  evidence; run it and record the exact command and a concise observed
  result.
- Corrections are appended, not rewritten. Failed attempts stay visible.
- Link large outputs or artifacts; don't paste blobs into the plan.
- Distinguish observed evidence from reported or historical claims.

## Lifecycle

1. Author under `plans/active/`, conforming to this file.
2. Execute milestone-by-milestone under the rules above.
3. On completion: write Outcomes & Retrospective, reflect any durable
   behavioral outcome into `docs/specs/` (a completed plan is an archive of a
   change, not a living description of current behavior), graduate durable
   decisions to `docs/decisions/`, then move the file to `plans/completed/`.
