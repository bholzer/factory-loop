# 0013 — Changing the plan convention triggers a conformance audit of every active plan

## Decision

Editing `plans/PLANS.md` requires auditing each plan in `plans/active/`
against the new text and recording the audit's outcome in that plan's
Decision Log, whether or not anything had to change. The `doc-garden` skill
owns this check.

## Rationale

When this convention was replaced mid-flight, the plan then in flight had
been authored under the superseded text and nobody noticed for two
milestones. The audit eventually happened because a reviewer worried, and a
rule that fires on worry is not a rule.

The cost is one read of a file that is already in context when the
convention changes. The exposure without it is a fresh-context session
executing a plan whose shape the current convention no longer accepts, and
discovering that halfway through a milestone — the most expensive moment
available.

## Date

2026-09-17, post-milestone review of the v1 blueprint plan.

## Status

accepted
