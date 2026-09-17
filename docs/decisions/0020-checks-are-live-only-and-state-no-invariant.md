# 0020 — This project's checks live in tools/, are never copied into the payload, and state no invariant of their own

## Decision

The executables that decide this repository's invariants live under `tools/` —
the aggregator, one check per card, the allowlists, and the hook. Nothing there
is copied into `template/`. A check contains no statement of the property it
decides: that belongs to the card in `docs/capabilities/`, and the script is
only the decision procedure plus the failure text the card publishes.

## Rationale

`GOALS.md` names stack-specific enforcer implementations as a non-goal, and the
reason is portability rather than tidiness. A directory of shell scripts copied
into the payload would arrive in a target project already assuming this
repository's file set — its artifact names, its two halves, its allowlist paths
— and would fail on the first run in a project that legitimately has none of
them. That is exactly the coupling the payload exists to avoid, and the
transferable part of a check is already shipped: the card states the invariant,
the failing case, and the remediation text, and a target project builds the
mechanism in whatever it actually runs.

Keeping the scripts outside the repository entirely was the other alternative,
and it contradicts the rule that nothing material lives outside the repo: a
check nobody can read is a gate nobody can review.

The split between script and card is what keeps a status claim meaningful. If a
script stated its own invariant, a reader comparing it against the card would
have two specifications and no way to tell which one the register's status is
about.

## Date

2026-09-17, graduated from the plan that built this repository's gate set.

## Status

accepted
