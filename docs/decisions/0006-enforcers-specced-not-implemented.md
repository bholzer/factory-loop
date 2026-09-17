# 0006 — Mechanical enforcers are specified as cards, never implemented in the payload

## Decision

Every invariant the blueprint wants enforced ships as a stack-agnostic
capability card stating the invariant, the enforcement point, observable
acceptance including a failing case a builder can actually make, a mandatory
remediation message, and optional per-stack hints. The payload contains no
linter, hook, or pipeline configuration. This binds the payload; checks this
repository wants for its own artifacts may be built here.

## Rationale

Mechanisms are stack-specific and the blueprint is stack-agnostic. Shipping
a pre-commit hook written for one ecosystem into a project using another is
worse than shipping nothing, because it has to be understood and deleted
before it can be replaced. A card instead guides an in-project agent that
can see the actual stack, which is the one participant with the information
the mechanism needs.

The mandatory remediation message is what stops a card from degrading into a
wish. A failure message is the only documentation an agent reads at the
moment it is wrong, so "boundary violation" is a failed check even when the
detection is correct: it names no offender and no next action.

## Date

2026-09-16, v1 blueprint design conversation.

## Status

accepted
