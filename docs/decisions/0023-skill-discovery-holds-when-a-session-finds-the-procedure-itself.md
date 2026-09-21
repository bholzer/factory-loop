# 0023 — Skill discovery is judged by whether the session found the procedure itself, not by whether the harness surfaced it

## Decision

The first of the five observations in `docs/capabilities/blueprint-eval.md`
holds when a session handed only the trial repository and the harness's own
default configuration names and then follows the right procedure for its task,
with nothing created or edited anywhere to make the procedures visible. The
operator's prompt may not name a procedure, nor any filename or path among the
artifacts that arrived in the copy; the target project's intended layout is
owner input and may appear freely. A flag granting the harness permission to
write or to reach the network is recorded with the invocation and not counted
against the observation.

## Rationale

The wording being replaced — that the harness discovered every skill — reads
naturally as "surfaced automatically by a loader", and under that reading the
trial answers a question this project has deliberately parked. Where the
procedures should live is the open item `docs/DEBT.md` tracks as `D4`, with
three candidate answers and no pick; an observation that fails a harness for not
having made the pick turns a parked question into a verdict, and a verdict
reached by accident.

The invariant the card actually states is that the copy is sufficient to carry
one feature. A session that reaches a procedure through the map has been
carried, and the map is the mechanism the payload ships on purpose. So the
property worth observing is whether a route existed that did not require a human
to configure something first.

Leaving the criterion unstated was the other alternative and it was the worst
one: three harness runs would each have judged it differently, and the records
would not have compared. The failing case still bites under this reading — with
the entry-point file renamed, no route reaches the bootstrap procedure by the
name every harness expects, which is the case the card asks someone to
demonstrate.

What that demonstration then found is worth carrying with the criterion: every
harness in the first trial reached the procedures by reading the tree, so a
session can also reach a renamed file by reading the tree. The criterion is
sound; the failing case built on a filename is the part that turned out weak.

## Date

2026-09-21, graduated from the plan that ran the first live trial.

## Status

accepted
