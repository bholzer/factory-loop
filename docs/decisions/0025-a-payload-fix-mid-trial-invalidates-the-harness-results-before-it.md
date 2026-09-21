# 0025 — A payload fix landing mid-trial invalidates every harness result recorded before it

## Decision

When a live trial is under way and a defect it found is fixed under `template/`
or `skills/`, every harness result already recorded against the text that
changed stops counting. The work that lands the fix also re-drives the affected
sessions, or labels each stale record as describing a payload that no longer
exists and names what is unverified. A fix is scoped to the sessions it touches:
correcting the bootstrap procedure invalidates bootstrap records and leaves
records of authoring and execution standing.

## Rationale

`docs/capabilities/blueprint-eval.md` makes the trial the gate on any change
under either half. A payload edit made in the middle of a trial is therefore the
exact class of change the gate exists to judge, and keeping a passing record
from before the edit claims a result about a payload nobody has run.

The cheap alternative is to treat the fix as an improvement and the record as
still broadly true. It fails on the only question the trial exists to answer:
whether the payload as it stands carries a feature. "Broadly true" is what
reading the payload from inside this repository already gives, and that is the
state the whole trial was built to escape.

Re-driving costs a session per invalidated record, which is why the rule also
permits the label. The label is not free either — it converts a result into an
open question and says so in the record — and choosing it over a re-run is a
judgement about how central the changed text was to the record.

The scoping clause is what keeps the rule from swallowing the trial. Without it,
any edit anywhere in either half would retire every record, and no trial with a
finding in it could ever complete.

## Date

2026-09-21, graduated from the plan that ran the first live trial.

## Status

accepted
