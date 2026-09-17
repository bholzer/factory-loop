# 0001 — The payload lives in `template/`; the repository root is its live instantiation

## Decision

Blueprint artifacts ship as a payload directory, `template/`, whose contents
are copied into a target project and filled in there. This repository's own
root artifacts are that same payload filled in for this project, which makes
this repository client #1 of its own bootstrap flow.

## Rationale

The obvious alternative — keeping the template at the repository root —
collides file for file with the `AGENTS.md`, `GOALS.md`, and `plans/` this
repository needs in order to operate under its own convention. A project
cannot simultaneously be its own skeleton. Moving the payload into a
subdirectory costs one copy step at bootstrap and buys the only routine
exercise the payload gets before a target project sees it: filling the
skeletons for this project. A skeleton that cannot be filled coherently is
then caught here rather than in someone else's repository, and divergence
between the two halves is a bug with a known location.

## Date

2026-09-16, v1 blueprint design conversation.

## Status

accepted
