# 0014 — Capability cards are project content, so they are excluded from template ↔ live counterpart checking

## Decision

The payload ships a starter set of capability cards, and a project prunes it:
a card that does not apply is deleted along with its register row, and the
omission is recorded in `GOALS.md` under scope. Card files under
`template/docs/capabilities/` therefore need no live counterpart at the same
relative path, and the correspondence check excludes them — as it excludes
`.gitkeep` placeholders. `docs/capabilities/index.md` is not excluded: it is a
skeleton and has a counterpart.

## Rationale

The alternative was to keep correspondence total by instantiating every
shipped card in the live tree. It fails on honesty: a card at `specced` status
is a claim that this project wants that enforcer, and `isolated-env` in a
repository with no toolchain or runtime is a check that would pass on
everything. Manufacturing a register row to satisfy a structural rule is
exactly the failure the card format warns about, and the rule would have been
purchased with a lie.

The second alternative was to ship no cards in the payload and let every
project write its own. That throws away the most transferable thing the
blueprint has: five invariants that hold in nearly any repository, already
written with their failing cases and remediation text. A project that starts
from a blank capabilities directory writes zero cards.

The cost of the exclusion is that the correspondence check can no longer
answer "should this live card have had a template counterpart", which is a
question it could never answer for `docs/decisions/` records or plans either.
The exclusion is narrow, mechanical, and stated where the check is specified.

## Date

2026-09-17, v1 blueprint design conversation.

## Status

accepted
