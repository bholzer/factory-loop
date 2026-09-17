# 0017 — A check is enforced here by the cheap command and the versioned pre-commit hook, never by continuous integration

## Decision

A capability card in this project reaches `enforced` when its check runs from
`./tools/verify` and from `tools/hooks/pre-commit`, the hook that runs that same
command and refuses the commit when it exits nonzero. No card here names a
continuous-integration job. The ways a committer can still get past the hook are
recorded in `docs/DEBT.md` as `D8`, and no rung on the ladder in
`docs/MATURITY.md` may be claimed while that row stands.

## Rationale

This repository has no remote and no job runner: measured while the gate set was
being planned, `git remote -v` printed nothing and git's hook path was unset.
`docs/capabilities/CARD_FORMAT.md` accepts a pre-commit hook or the project's
cheap verification command as an enforcement point, so the status is honest as
written.

The alternative was to keep the phrasing the payload's generic cards use, which
names continuous integration alongside a local command. It would have published
a gate nobody can observe firing, and the promotion rule in `docs/MATURITY.md`
reads the register as fact — a rung would then rest on a job that does not
exist. The second alternative was to leave every card at `built` until somewhere
unskippable exists to run it. That reads as though nothing runs, when in
practice the checks run on every commit in every clone that has executed one
config line.

The cost is real and is the reason `D8` exists rather than a footnote: a flag
skips the hook silently, a fresh clone has no hook until someone installs it,
and the hook judges the working tree rather than the staged content. The first
two weaken the gate; the third errs toward refusing too much. A card claiming
more than that would be the failure this record prevents.

## Date

2026-09-17, graduated from the plan that built this repository's gate set.

## Status

accepted
