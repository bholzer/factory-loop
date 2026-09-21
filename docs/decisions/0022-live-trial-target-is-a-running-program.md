# 0022 — The live trial's target project is a small running program, never another document system

## Decision

A run of the trial specified in `docs/capabilities/blueprint-eval.md` bootstraps
a target project that has a toolchain: code that executes, a test command that
does not exist at bootstrap, a state path two checkouts could collide on, and a
layering rule a grep can decide. The target uses its host language's standard
library only — no dependency fetch, no network, no installation step — and the
same target is used in every harness so that the three records compare.

## Rationale

The obvious cheaper target is a documentation repository, because this
repository is one and the payload was written against one. It was refused. Three
of the cards the payload ships — the cheap verification command, environment
isolation, and the dependency-direction check — say nothing about a tree of
markdown: one of them was never instantiated here at all, for the reason
`GOALS.md` records under Scope, and the others were filled against a subject
with no runtime. A trial against a second document system would have exercised
the artifact layer twice and the toolchain-facing cards zero times, which is
precisely the part nobody had evidence about.

The standard-library constraint is not fastidiousness. A harness driven
non-interactively may run inside a sandbox that denies network access, and a
target needing a package install would then fail the trial for a reason the
payload has nothing to do with — an unattributable red result, which is worse
than no result.

One target across all harnesses is what makes the runs readable against each
other. The three independent projects chose three different names for their
stored data and three different published verification commands, and answered
identically at the terminal; that comparison exists only because the brief they
were given was the same brief.

## Date

2026-09-21, graduated from the plan that ran the first live trial.

## Status

accepted
