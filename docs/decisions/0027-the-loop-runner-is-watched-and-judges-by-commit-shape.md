# 0027 — The loop runner is started and watched by a person, and it judges an iteration from commits rather than from what the session said

## Decision

`tools/loop-runner` drives milestone sessions only when a person starts it,
only for the iteration count that person supplies, and only up to the plan
boundary that person then reads; building it and enforcing its stop rule
claims no rung on the ladder in `docs/MATURITY.md`. What the runner decides
about each iteration comes from git — the commits the iteration left, whether
each of them touched the plan being advanced, and which Progress entry changed
state between the revisions on either side — and never from the session's own
account of itself. A session that exits nonzero halts the run even when its
commits look clean.

## Rationale

The alternative to watching is the thing the card's invariant is written for:
an unattended loop, scheduled or simply left running. It is refused on two
counts. The promotion rule in `docs/MATURITY.md` would have to be satisfied
first — twenty consecutive green landed changes behind a gate the committer
cannot switch off, and `docs/DEBT.md` `D8` records that this repository's gate
is switchable — so an unattended claim would rest on a rung nobody may claim.
And the failure modes of iteration are cheapest to meet while somebody is
looking at them: the halts this runner recognises were all first seen by a
human running the loop by hand, and none of them was predicted before it was
observed. A loop running unwatched is the most expensive place to discover the
next one.

The alternative to judging by commit shape is to believe the session. Two
kinds of evidence say not to. Exit status is wrong in both directions: one
supported harness printed a two-line refusal about credits, made no commit,
and exited 0, while another exits nonzero for account state, for a model
version it will not run, and for a directory it does not trust — the last of
those before any model is called at all. And the session's closing report is
prose written by the thing being judged; a harness that streams nothing until
the end leaves a log that is one report long, which is enough to read after a
halt and not enough to decide one. Commits are the only account of an
iteration that the iteration cannot revise.

Judging from the plan's record as well as from the commit list is what makes
the judgement bite. A self-test fixture built for this record — a session that
commits real prose to the plan and ticks nothing — passes every commit-shaped
test and is exactly the case the runner exists to stop, because the next
iteration would then start on top of an unrecorded one. It was the only
fixture that caught a deliberately broken runner; the four that assert
commits, touched paths and exit codes all passed against it.

Halting on a nonzero exit even when the commits are clean is the conservative
edge of the same argument. A session that recorded its milestone and then died
has left a record whose author did not finish; the run stops and a person
reads it. The cost is a halt that a more permissive rule would have skipped
past, and that cost is what the operator asked for in choosing a watched loop.

## Date

2026-09-22, graduated from the plan that built the runner, its self-test, and
the first watched run against a real harness.

## Status

accepted
