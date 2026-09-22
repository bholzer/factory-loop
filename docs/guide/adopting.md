# Adopting

This page is the walkthrough for bringing the blueprint to a project of
your own: what to have in hand, how to seed the target repository, how to
run the single bootstrap session, and what a finished bootstrap looks
like. It describes the flow from the adopter's chair; the procedure the
session itself follows is `skills/harness-init/SKILL.md`, and every
observed claim below — what real runs did, how long they took — is owned
by `docs/specs/bootstrap-flow.md`, which wins wherever this retelling and
that record disagree. The mental model behind the pieces is
`docs/guide/overview.md`.

## What you need

Three things. A checkout of this repository: the copy below reads from
it, and afterwards the target never touches this tree again. A target
repository: empty is the easy case, and the recorded trials have so far
covered only that case — before bootstrapping an existing codebase whose
declared layering is already being violated, read `D6` in `docs/DEBT.md`,
because the dependency-direction card has no retrofit story yet and a
bootstrap must not pretend it does. And one harness, meaning the agent
product that will drive the session; `docs/specs/bootstrap-flow.md` names
the three this project has recorded runs in.

## Seed the target

Installation is a copy, not a program: nothing generates, nothing
executes, and the target gains no dependency. Two directories travel. The
contents of `template/` — the payload — land at the target's root, so the
file shipped as `template/AGENTS.md` arrives as the target's own
`AGENTS.md`, and so on across the artifact set. The `skills/` directory —
the six procedures — is copied beside them, whole and unedited.
`docs/specs/bootstrap-flow.md` owns this description of what an outside
reader receives: a plain recursive copy, then an editing pass that the
bootstrap session performs in place. Commit the copy as it arrived,
before any session touches it; every later edit then reads as a diff
against the payload as shipped.

## Run the bootstrap

Start one session in the target and tell it to find the bootstrap
procedure in the tree and follow it, with the owners' answers supplied in
the same prompt or kept ready at the terminal. Do not worry about
pointing the harness at the file: in the recorded trials no harness
surfaced the arriving procedures to its session at startup, and every
session located `skills/harness-init/SKILL.md` by listing the repository,
inside its first two commands — `docs/specs/bootstrap-flow.md` carries
that observation. Discovery is the cheap part.

What the session needs from the owners is the interview in the
procedure's step 2, asked in one pass: the project's purpose; who or what
it serves; which outcomes count as success; the deliberate exclusions
from scope; the non-goals someone will otherwise propose; the constraints
that outrank convenience. An answer nobody has becomes a recorded unknown
— the procedure treats an invented fact as worse than a missing one. So
an unattended run's driving prompt is simply those answers written out,
and an attended run keeps the owners reachable instead.

Then let it work. The procedure right-sizes the artifact set in the open
— what shrinks is project content such as the starter card register,
never the structure — fills every skeleton, installs the procedures
unmodified, and stops before the project's first real change: no first
feature, no first plan.

## What to expect

One committed installation, then a report. Steps 9 through 11 of
`skills/harness-init/SKILL.md` bind the ending: the session verifies its
work by running it rather than reading it, commits the installation, and
reports — what was created; which content came from interview answers
rather than from the tree; what was omitted, with the reason per
omission; what was deferred into the target's own debt register; and how
each of step 9's two clauses came out. Those clauses are the entry-point
file added when the environment does not read `AGENTS.md` on its own, or
the reason none was needed, and what was installed to make the procedures
reachable, or why nothing was.

Reachability is the part that varies by harness, and it has its own rule:
the installed procedures keep their directory and entry-point name, never
moving to suit an environment, and each harness gets reachability its own
way — configuration the repository itself carries, a committed link, or
nothing at all when the locations that harness reads all sit outside the
repository, because a bootstrap configures a repository and not a
machine.
`docs/decisions/0026-installed-procedures-stay-put-and-reachability-is-installed-per-harness.md`
states the rule, and `docs/specs/bootstrap-flow.md` records all three
outcomes observed across the supported harnesses, link locations
included.

A greenfield target adds one expected wrinkle. The artifacts the session
just filled describe code nobody has written yet — the stated layering,
the module paths, the directories the first plan will bring into being —
so the reference walk in the procedure's step 10 does not come back clean
there, and is not supposed to: those paths are reserved, not broken, and
the honest form of that judgement is the debt row the bootstrap opens to
carry them. A report that presented the walk as clean would be the
failure; the row is the pass.

For sizing, the recorded figure in `docs/specs/bootstrap-flow.md` is
twenty to twenty-nine minutes of unattended driven time per harness for
the full three-session sequence the trials ran — bootstrap, then a first
plan, then that plan's first milestone — the bootstrap being one of the
three.

## After the bootstrap

The bootstrap ends with an installed knowledge layer and stops there; the
stop is written into the procedure, so what follows are new sessions, not
a continuation. The project's first plan is authored in a session of its
own under `skills/plan-author/SKILL.md`, against the plan convention the
payload installed at `plans/PLANS.md`; that plan's first milestone is
executed in another session under `skills/plan-execute/SKILL.md`. The
recorded trials ran exactly this sequence in each harness, and what they
produced — plus what has deliberately not been observed yet, damaged
procedure sets and brownfield targets among it — is
`docs/specs/bootstrap-flow.md`'s closing word.
