# 0026 — Installed procedures keep their location and each harness gets reachability installed into the repository, in one of three named ways

## Decision

A bootstrapped project keeps its procedures at `skills/<name>/SKILL.md` and
never moves them to suit a harness. Reachability is something the bootstrap
installs into the repository it is running in, and step 9 of
`skills/harness-init/SKILL.md` names the three outcomes exhaustively: where the
harness reads its own configuration from inside the repository, that
configuration makes the set reachable; where it reads a location inside the
repository that the project does not ship, a link is created there and
committed with the rest of the installation; and where every location it reads
lies outside the repository, nothing is installed, because a bootstrap
configures the repository it runs in and never the machine it runs on.
Whichever of the three happened is named in the report the procedure ends
with.

## Rationale

The alternative that looks obvious is to configure each harness to scan the
repository's `skills/` directory. It costs a procedure body that names
configuration files and keys belonging to particular harnesses, which the
`no-unprovided-skill-target` rule in `ARCHITECTURE.md` forbids — those paths
are ones the payload does not ship — and which
`docs/decisions/0008-portability-lowest-common-denominator.md` already decided
against. It also cannot be completed: one of the three supported harnesses
reads only roots outside any repository, so no in-repository configuration
reaches it at all.

The second alternative is to move the canonical location into whichever dotted
directory the harnesses share. No such directory exists. Three trials watched
link-writing sessions choose two different dotted directories, and the third
harness read its own system and plugin caches under the machine owner's home.
Moving would break the map line the bootstrap writes, the cross-references
inside all six procedures, and the allowed-target list in `ARCHITECTURE.md`'s
layer map, in exchange for a root that one harness in three reads.

What remains is the candidate that already ships, and it is the only one with
a controlled measurement behind it: in each harness probed on both sides, a
bootstrapped copy carrying the link named all six procedures at startup where
the same copy without it named none. Three trials against this wording then
produced all three outcomes — two links under different dotted directories,
and one install-nothing in the harness the third branch exists for — which is
the evidence that the rule covers the space rather than describing the case
someone happened to meet first.

Naming three outcomes rather than giving one instruction is the part that
costs a sentence and is worth it. A session in a harness whose roots lie
outside the repository must otherwise derive unaided that writing into them
configures the machine instead of the project; one session did derive it, and
the next one may not. The reporting obligation exists because the same harness
resolved the old open-ended clause two different ways on two runs and left no
record of either, so no later reader could tell a deliberate choice from an
accident. One harness in three still answered the obligation with silence when
it had nothing to install, which `docs/DEBT.md` carries as a wording defect
with its own trigger rather than a reason to reopen the location question.

This record closes the open item that
`docs/decisions/0023-skill-discovery-holds-when-a-session-finds-the-procedure-itself.md`
names in its Rationale as parked: where the procedures should live, which that
record deliberately refused to let a trial observation settle by accident. It
is settled here, by a decision, and the criterion `0023` states is unaffected
— a session still satisfies it by finding the procedure itself, whether or not
its harness was handed one.

## Date

2026-09-22, graduated from the plan that decided the location and re-ran the
live trial in all three harnesses.

## Status

accepted
