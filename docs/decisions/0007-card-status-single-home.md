# 0007 — Capability card status lives only in the capabilities index

## Decision

A card's status — `specced`, `built`, or `enforced` — is recorded in
`docs/capabilities/index.md` and nowhere else; card files carry no status
field. The demonstrated failing case that promotes a card to `built` is
recorded in the plan that built it, not in the card.

## Rationale

Status is the one fact about a card that changes, and a changing fact with
two homes goes stale in one of them. The index is what the maturity ladder's
gates read, so the index wins and the cards stay specifications.

Keeping promotion evidence in plans preserves that property. A card is a
one-page contract an in-project agent implements cold; a card that
accumulates observation transcripts stops being readable at a glance, and
the evidence itself is a historical record of one change, which is precisely
what a plan is for.

## Date

2026-09-17, settled while writing the capability-card format.

## Status

accepted
