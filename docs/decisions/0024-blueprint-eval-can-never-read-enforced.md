# 0024 — The live-trial card's status ceiling is `built`; it can never read `enforced`

## Decision

`blueprint-eval` may be recorded in `docs/capabilities/index.md` as `specced`
or `built` and never as `enforced`, however well a trial goes. A later session
that wants the L2 row in `docs/MATURITY.md` satisfied must change the table or
split the card, and may not reach the status by promoting this one.

## Rationale

`docs/capabilities/CARD_FORMAT.md` defines `enforced` as running where it cannot
be skipped. Three quarters of this card's acceptance is a person invoking an
agent harness and reading what came back — five observations, of which only the
scaffolding check and the reference walk are decided by a program. No trigger
can make that unskippable, because the thing it triggers is a human judgement.

The tempting alternative is to promote on the scriptable quarter, which does run
on demand and does catch what it claims. It loses because a card states one
invariant and carries one status: a register reading `enforced` beside an
invariant three quarters of which nobody automated is the manufactured green
check the format document warns about, and the first reader to trust it would be
trusting a gate that is not there.

Writing the ceiling down rather than leaving it implied has a specific purpose.
The promotion rule in `docs/MATURITY.md` asks every gating card of a rung to read
`enforced`, so this row blocks an L2 claim permanently, and the cheapest way out
of that bind — edit the status — is exactly the move this record forbids. The
honest exits are to gate on the scriptable part alone, or to retire the row and
name the human judgement that stays.

## Date

2026-09-21, graduated from the plan that ran the first live trial.

## Status

accepted
