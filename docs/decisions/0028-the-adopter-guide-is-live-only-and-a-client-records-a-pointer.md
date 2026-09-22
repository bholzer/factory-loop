# 0028 — The adopter guide is live-only and a client records a pointer to it

## Decision

The role-and-task guide under `docs/guide/` exists only in this repository:
the payload ships no copy, and no file a target project receives names it.
A project that wants the guide reachable records where its artifact set
came from — the optional provenance question in step 2 of
`skills/harness-init/SKILL.md` — as a line in its own `GOALS.md` naming the
upstream checkout and the guide location inside it, both outside that
project's tree.

## Rationale

The obvious alternative is to ship the guide with the payload, so every
bootstrapped project carries its own manual. It loses on the layer map
before it loses on maintenance: `ARCHITECTURE.md`'s
`no-outward-payload-reference` rule forbids anything under `template/` to
mention this repository, and the guide's subject is this repository — its
trial records, its operating practice, its check protocol, the prompts its
operator types. A copy stripped of those subjects would be a pamphlet
about nothing, and a copy keeping them would arrive in every client with
references that dangle by construction. Maintenance seals it: five pages
revised in one place stay one revision, while a shipped copy is stale the
day after the first edit here and there is no update channel to a project
that was installed by a recursive copy.

The pointer costs almost nothing and fails soft. One optional interview
question, one line in the client's goals file, phrased as a location
outside the client repository so no checker built there is asked to
resolve it — the same judgement
`docs/decisions/0021-a-path-that-must-not-resolve-is-not-backticked.md`
records for paths here. Owners who decline lose a convenience, not a
capability: the payload and procedures arrive whole either way, and
`docs/guide/index.md` is written as the page such a recorded pointer lands
on.

## Date

2026-09-22, owner decision graduated from the plan that published the
guide and re-drove the bootstrap trials.

## Status

accepted
