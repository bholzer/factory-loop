# Maturity ladder

## Current rung

L0 — human-gated. Three checks are now mechanical: `./tools/verify` decides the
absence of authoring scaffolding in the live artifacts, the whole
correspondence between `template/` and the live tree — counterpart existence,
byte identity, and heading subsequence — and the resolution of every path
reference in the live artifacts; and the hook at `tools/hooks/pre-commit`
refuses a commit it rejects. Everything else here is still enforced by a human
reading, and a human reads every change before it lands.

The rung stays L0 although both L1 rows in the Gating capabilities table below
now read `enforced` in `docs/capabilities/index.md`. Two things are missing on
top of that status: the twenty consecutive green landed changes the promotion
rule requires, and a gate the committer cannot switch off, which
`docs/DEBT.md` `D8` records this one as not being. Of the L2 rows,
`doc-integrity` reads `enforced` too, while `blueprint-eval` needs a live
trial this project has deliberately parked and `loop-runner` gates a rung two
steps away.

## Rungs

Four rungs, from fully human-gated to bounded autonomy. Each states what the
agent may do unsupervised, what still requires a human, and what evidence
every change carries. The definitions are deliberately concrete enough that
two people looking at a landed change agree on which rung it was performed
under.

### L0 — Human-gated

The agent may read, plan, implement, run whatever checks exist, and revise its
own work. It may not merge. A human reads every change before it lands, and
approves every edit to `GOALS.md`, `docs/PRINCIPLES.md`, `ARCHITECTURE.md`,
and every capability status change.

Evidence every change carries: the plan's living sections updated with output
the agent actually observed, including the commands it ran and what they
printed.

### L1 — Self-verifying

A cheap verification command exists and is enforced, so "this works" is a
claim a human can re-check in one command instead of by reading. The agent may
land changes whose entire risk surface is covered by enforced checks; anything
outside that surface — public interfaces, the knowledge artifacts named at L0,
anything with no check behind it — still needs a human.

Evidence every change carries: the verification command's own output, recorded
in the plan, plus the statement of which parts of the change the enforced
checks do not cover.

### L2 — Agent-reviewed

A reviewing agent reads the change before a human does, and reviews against
the artifacts — principles, architecture, the card set, the plan's acceptance
— rather than against taste. The human reviews escalations and a sample. This
rung requires the structural checks to be enforced, because a review pass is
otherwise the only thing between drift and the repository.

Evidence every change carries: the review record, naming what it checked and
what it could not check, alongside the verification output from L1.

### L3 — Bounded autonomous classes

For named classes of work, the agent plans, implements, verifies, and merges
with no human in the loop. The classes are listed explicitly and their
boundary is mechanically decidable — a change touching a file outside a
class's allowed set is not in that class and falls back to the rung below.
There is no general autonomy at L3, only enumerated autonomy.

Evidence every change carries: the L1 and L2 evidence plus a per-class audit
trail a human samples at a stated cadence, which is what makes the boundary
claim checkable after the fact rather than at the moment of merge.

## Promotion rule

Trust is transferred to machinery, never to reputation. A rung may be claimed
only when every card in the Gating capabilities table for that rung has status
`enforced` in `docs/capabilities/index.md`, and every one of them has been
green for twenty consecutive landed changes with no gating check disabled,
skipped, or made advisory during that run. A named human confirms the claim in
the commit that edits the Current rung section above.

"The agent has been doing good work" is not an input to this rule. The
question is never whether the agent is trustworthy; it is whether the specific
mistake the human gate was catching is now caught by something that runs every
time.

## Demotion rule

The current rung drops by one immediately on any of three triggers: a failure
escapes that a gating card was supposed to catch; a gating card leaves
`enforced` by being disabled, skipped in continuous integration, or made
advisory; or a gating check is spot-checked against a violation and fails to
fail.

Re-earning the rung requires more than restoring the previous state. The gate
must first be extended to catch the case that escaped, demonstrated with the
failing case its card requires, and the green-cycle count then restarts from
zero. The drop and the reason are recorded in the Current rung section, since
a ladder with no record of falling reads as one that only ever rose.

## Gating capabilities

| Rung | Gating card | What it must subsume |
| --- | --- | --- |
| L1 | `fast-verify` | a human remembering to run this repository's checks by hand after every edit |
| L1 | `template-live-drift` | a human walking `template/` against the live tree to find divergence |
| L2 | `doc-integrity` | a human noticing that a map line or a cross-reference names a file that does not exist |
| L2 | `blueprint-eval` | a human judging that a payload change still bootstraps a working project in every supported harness |
| L2 | `loop-runner` | a human invoking each milestone session and deciding after each whether iteration continues |

Rows name only cards in this project's own register. L3 has no row, because
the check that decides whether a change falls inside an autonomous class has
to be written against named classes this project does not yet have.
