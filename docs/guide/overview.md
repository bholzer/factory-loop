# Overview

This page is the mental model: what the pieces of this repository are, why
each exists, and how they combine into a working loop. It is a tour, not an
owner — every fact here lives in some other file, named beside the passage
that leans on it, and the named file is the one to trust when the two
disagree.

## What this is

This repository is a blueprint for agent-operated development: a set of
repository artifacts plus portable procedures that let a coding agent
bootstrap, run, and maintain its own working environment inside a project.
The premise is that an agent session starts empty. Nothing said in an
earlier conversation exists anymore, so the repository itself must carry
everything the work needs — goals, constraints, plans, evidence — and
anything decided outside it must land in it to exist at all. Every
artifact is markdown, the only tools assumed are shell and git, and none
of it presumes a particular stack or harness, "harness" meaning the agent
product that drives a session. `GOALS.md` owns the purpose, the five
subsystems the blueprint encodes, and the non-goals; `docs/PRINCIPLES.md`
owns the few rules that outrank local convenience.

## Two halves, one direction

The tree splits in two, and the split is the architecture. `template/` is
the payload: the artifact set a target project receives as a plain
recursive copy and fills in where it lands — skeletons carrying fill
slots, format documents, the plan convention, a starter register of
capability cards. Everything at the repository root is that same set
already filled in for this project, which makes this repository the first
client of its own payload and makes divergence between the two halves a
bug in one of them.

What keeps the halves coherent is the direction of reference, owned as
explicit rules by `ARCHITECTURE.md`'s layer map. The payload may not name
anything outside itself, because the project receiving it has no ancestor
context for an outward path to resolve in. The procedures under `skills/`
may name only what the payload ships, because they travel with it. The
live root may discuss `template/` freely — the payload is this project's
subject matter — and that is the only direction in which the two halves
touch. A last rule protects the boundary around `plans/`: no file outside
it may depend on a specific plan file, so the living description of the
system never leans on the archive of how it got that way.

## Six procedures, six occasions

`skills/` holds the reusable procedures, each one markdown file an agent
reads and follows, written over files, shell, and git so the same text
works under any harness. Each exists for one occasion.

- `skills/harness-init/SKILL.md` — a repository that has none of this
  yet. The bootstrap: it orients in whatever is already there, interviews
  the owners for what no file can supply, right-sizes the artifact set,
  fills every skeleton, installs the procedure set, and reports what was
  created and what was deliberately omitted.
- `skills/plan-author/SKILL.md` — work about to span more than one file
  or one session. It produces the plan that work will run under and stops
  before implementing any of it.
- `skills/plan-execute/SKILL.md` — a plan with an unfinished milestone. A
  fresh session executes exactly that milestone, records its evidence,
  and stops.
- `skills/capability-build/SKILL.md` — a capability card whose check
  nobody has built. It turns the card into a working check in the
  project's own stack and proves the check by watching it fail.
- `skills/doc-garden/SKILL.md` — a cadence rather than an event: sweep
  the knowledge artifacts for drift, then land the smallest repair each
  finding needs.
- `skills/retro/SKILL.md` — a finished piece of work. It reconstructs
  what the work intended against what it actually did and routes each
  material lesson to exactly one owning artifact.

## A plan is the context

Work that spans files or sessions runs under an ExecPlan: a design
document written so a reader with nothing but the plan file and the
worktree can carry the work forward. `plans/PLANS.md` is the convention
that defines them and the bar their evidence is judged against; the shape
exists for the fresh-context loop. A session's context window is treated
as a disposable cache — nothing in it survives — so the plan file is where
the state of the work actually lives. Each session reads the plan,
executes one milestone sized to fit a single session, writes what it
observed into the plan's living sections, and stops; the next session,
with no memory of the last, resumes from what was written. The record
holds observed evidence only: a command nobody ran proves nothing,
whatever its expected result. Work a session notices but must not absorb
is deferred to the debt register in `docs/DEBT.md` rather than done on
the side.

Plans in flight live under `plans/active/`; finished ones move to
`plans/completed/`, where they are archives of a change rather than
descriptions of the present. Before that move, whatever the plan changed
about observable behavior is reflected into a spec under `docs/specs/` —
`docs/specs/index.md` routes which file owns what — and decisions with
lasting force graduate to one short record each under `docs/decisions/`.

## From invariant to enforcement

A rule that matters is not left to review comments. It is written as a
capability card under `docs/capabilities/` — one page, one invariant, in
the shape `docs/capabilities/CARD_FORMAT.md` defines: the property that
must always hold, the point where the check runs, acceptance stated as
both a passing and a failing case, and the exact text a failure prints —
mandatory, because an agent meets that text at precisely the moment it is
doing the wrong thing, which makes it the one piece of documentation
certain to be read. A card specifies; the mechanism belongs to whoever
builds it, in whatever stack the project happens to have.

A card's status is one of three words — specced, built, enforced —
recorded only in the register at `docs/capabilities/index.md`. The middle
transition is the load-bearing one: built may not be claimed until
someone has introduced a real violation and watched the check fail with
its remediation text visible, because a check never seen to fail is not
known to decide anything. Enforced means the check runs somewhere it
cannot be skipped. In this repository every check also conforms to one
shared contract — exit meanings, output shapes, allowlist format — owned
by `docs/specs/check-protocol.md`.

Enforced checks are what the autonomy ladder stands on. `docs/MATURITY.md`
defines four rungs, from every-change-human-read to autonomy over named
classes of work, and its promotion rule transfers trust only to
machinery: a rung is claimable when each card gating it is enforced and
has stayed green across a stated run of landed changes, and the rung
falls the moment a gating check is skipped, disabled, or caught failing
to fail. The question is never whether the agent has been doing good
work; it is whether the specific mistake a human gate was catching is now
caught mechanically, every time.

## The loop that runs it

Day to day the system is one loop with a machine at the small end and a
person at the large end. After any edit, the cheap verification command
decides everything this repository has made mechanically decidable:
`./tools/verify`, named with its time budget under Commands in
`AGENTS.md`, runs each check in a fixed order, streams every check's own
report, and fails if any of them finds a violation or cannot decide.
`docs/capabilities/fast-verify.md` is the card holding that invariant,
and `docs/specs/mechanical-checks.md` describes the commands a reader can
run and what each check decides. The versioned hook at
`tools/hooks/pre-commit` runs the same command and refuses any commit it
rejects, so an edit meets the checks before it can land.

One level up sits the watched runner, `tools/loop-runner`, which advances
a plan one fresh session per milestone and judges each iteration by what
its commits show, not by what its transcript says, halting the moment an
iteration leaves no advance behind; `docs/capabilities/loop-runner.md`
owns that stop rule. And at the top sits the human: every run is started
by a person, bounded by an iteration limit that person chose, and read at
a plan boundary, where the diff and the plan's living sections get the
review no check has yet subsumed. `docs/MATURITY.md`'s current rung
records precisely how much of the system still rests on that reading —
and the ladder is the instrument for moving it, one demonstrated check at
a time.

`AGENTS.md` is where a working session starts: a map of the tree, kept
deliberately small, pointing at everything this page just walked through.
