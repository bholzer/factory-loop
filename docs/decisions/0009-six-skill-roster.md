# 0009 — Six skills, one per edge of the loop

## Decision

The skill roster is `harness-init`, `plan-author`, `plan-execute`,
`capability-build`, `doc-garden`, and `retro` — bootstrap, plan, execute,
enforce, garbage-collect, meta-learn. Planning and execution stay separate
skills. A seventh skill has to show which edge it owns that none of the six
covers.

## Rationale

Skills are workflow entry points, and an entry point that overlaps another
turns invocation into a coin flip. One skill per edge makes the roster
enumerable and the routing obvious.

The plan-author / plan-execute split is deliberate rather than tidy:
authoring and implementing are separately authorized activities — a plan can
be written and reviewed without anyone agreeing that work should start — and
execution is where drift is most expensive, so its discipline deserves a
body of its own instead of a section inside something larger.
`capability-build` is separate from `harness-init` for the same class of
reason: initialization runs once per project, while building enforcers
recurs forever and is the edge that moves the maturity ladder.

## Date

2026-09-16, v1 blueprint design conversation.

## Status

accepted
