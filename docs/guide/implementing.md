# Implementing

This page is the builder's path: it begins at a capability card — a
one-page specification of an invariant somebody wants enforced by a
machine — and ends at a check that runs where skipping it takes a
deliberate act, with a register row recording exactly that. The steps
themselves belong to `skills/capability-build/SKILL.md`, the procedure a
building session follows wherever the payload has landed; what this page
adds is reading order, the wiring specific to this repository, and the
two records every build leaves. For why invariants become cards at all,
and what enforcement buys on the autonomy ladder, read
`docs/guide/overview.md`.

## Start at the card

Read the card whole before deciding anything. Its invariant is what the
check will decide, its acceptance is the proof owed at the end — a
passing case and a failing case, both observable — and its remediation
message is the text the finished check must print; what each of those
sections binds, and what a status transition demands, is defined by
`docs/capabilities/CARD_FORMAT.md`. A card that is missing, or whose
invariant needs judgement to apply, is not ready for this path: writing
or repairing the specification comes first, as its own change, because a
check built against a vague card decides nothing while appearing to.

The rest of the pre-reading is listed in
`skills/capability-build/SKILL.md` itself — the register for what is
already enforced and where, `docs/MATURITY.md` for the rung the card
gates, the project's real toolchain — and exists to steer the mechanism
choice toward extending something that already runs.

## Wiring it in this repository

Here a check is a single shell script under `tools/checks/`, and
everything about how it behaves as a citizen — invocation, exit
meanings, the shape of passing and failing output, the allowlist format
under `tools/allow/`, the budget rule behind the seconds `AGENTS.md`
publishes — is the contract owned by `docs/specs/check-protocol.md`.
Conforming is what makes the new check readable by everything that
already reads checks: the aggregator, the hook, and anyone meeting a
failure at the moment it blocks them.

Enrollment is one edit: `tools/verify` runs the checks named in the
`CHECKS` list written into its own text, so a new check runs when its
name joins that list and never before — deliberately not a glob, so
nothing can join or leave the run without a visible diff. The commit
gate needs no separate step, because `tools/hooks/pre-commit` invokes
the same command; joining the list is joining the gate.

## Building it anywhere else

A project bootstrapped from this blueprint receives the same procedure,
so the path in a client project is `skills/capability-build/SKILL.md`
followed against that project's own stack: the mechanism there is a
linter rule, a test, a CI job, a script behind whatever cheap command
the project publishes — whichever existing thing can decide the
invariant exactly. The card does not constrain that choice, and
`docs/capabilities/CARD_FORMAT.md` is explicit that it must not: cards
state what must hold and how to prove the checker works, never the
technology. The protocol spec above is this repository's local
convention, not payload; a client that wants one writes its own.

## The proof

No status may move on a check that has only been watched passing. The
transition to `built` requires the demonstration the card's acceptance
spells out: introduce the violating change it describes, run the check,
and observe the failure — nonzero exit, the card's remediation wording,
the real offender named inside it — then revert the violation and see
the pass return. `docs/capabilities/CARD_FORMAT.md` owns this promotion
bar, and `skills/capability-build/SKILL.md` walks it as a numbered step.
A check that will not fail on the violation it exists for is wrong, and
the card is not adjusted to agree with it.

## The two records

Each build ends by writing exactly two places. The evidence — the
violating change made, the command run, the failure text observed —
lands in the plan that authorized the build, because the card stays a
specification and never absorbs history;
`docs/capabilities/CARD_FORMAT.md` states where evidence lives. The
status — `specced` to `built` to `enforced` — changes in one place, the
register at `docs/capabilities/index.md`, together with the command or
hook it runs under, so there is a single row to learn what is protecting
the tree and a single diff when that changes.
