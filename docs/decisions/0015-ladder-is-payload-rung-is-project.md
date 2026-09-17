# 0015 — The maturity ladder ships filled; only a project's position on it is a fill slot

## Decision

`MATURITY.md` ships with its rung definitions, promotion rule, and demotion
rule written as final content. What a bootstrapped project fills in is its
Current rung, the green-cycle count if the default is wrong for its change
rate, and the Gating capabilities table, whose rows must name only cards that
exist in its `docs/capabilities/`.

## Rationale

The rungs and the promotion rule are the blueprint's opinion about how
autonomy is earned — trust transfers to machinery, never to reputation — and
an opinion left as a fill slot is an opinion not shipped. A project asked to
invent four rung definitions during bootstrap will either copy something
vague or skip the file, and a skipped ladder is how autonomy arrives by drift:
nobody decides the agent may merge unreviewed work, review just quietly
stops.

The alternative — leaving the whole file as slots, which is how it was first
drafted — was rejected for that reason. The opposite alternative, shipping the
file with no slots at all, was rejected because the current rung and the gate
table are the only parts that are actually about a particular project, and a
ladder that does not say where this project stands is decoration.

This keeps `MATURITY.md` in the skeleton class with exactly one fill slot, so
the mechanical "is this file filled in?" check still covers it.

## Date

2026-09-17, v1 blueprint design conversation.

## Status

accepted
