# loop-runner

## Invariant

An unattended run advances exactly one milestone per iteration and stops
rather than continuing when a milestone's acceptance was not observed. Stated
so that a violation is decidable from the run's commits:

- each iteration produces at least one commit, and every such commit touches
  the plan file it is advancing;
- across one iteration, exactly one Progress entry in that plan changes state,
  and if it became complete it carries a timestamp;
- no iteration begins while the previous milestone's acceptance is unrecorded
  in the plan;
- the run halts on the first iteration whose milestone it could not complete,
  leaving the plan's Progress split in place per the convention in
  `plans/PLANS.md`.

This is a stop-rule invariant, not a scheduling feature. The value of the
outer loop living outside the context window is that each iteration starts
fresh from the plan file; a runner that carries state between iterations, or
that pushes through a failed milestone, reintroduces exactly the drift the
one-milestone-per-session rule removes.

## Enforcement point

The runner itself, since nothing else can observe its own stop rule, checked
by a self-test over a fixture plan that runs in continuous integration
alongside the other checks. Nothing enforces it today:
`docs/capabilities/index.md` carries the status and `docs/MATURITY.md` names
the rung that requires it. `GOALS.md` records why it is parked for v1 — the
failure domains of unattended iteration are not yet understood, and a loop
that runs while nobody is watching is the worst place to discover them.

## Acceptance

Passing case: build a fixture plan with three trivially verifiable milestones
in a scratch repository, run the runner with an iteration limit of two, and
observe exactly two milestones advanced, two commits each touching the fixture
plan, each advanced Progress entry carrying a timestamp, the third milestone
untouched, and a final summary naming what it did.

Failing cases, both required before the card is `built`:

- Make the second milestone's acceptance unobservable — its command exits
  nonzero. Run with a limit of three and observe the runner stop after the
  second iteration with a message naming the plan, the milestone, and what it
  could not observe; observe that the third milestone was not started and
  that Progress shows the split state.
- Point the runner at a plan whose living sections are stale — a completed
  Progress entry with no timestamp, or a missing required section — and
  observe it refuse to start at all, naming the plan and the defect. The
  structural contract it defers to is the one specified in
  `template/docs/capabilities/evidence-check.md`.

## Remediation message

    loop-runner: halted after iteration 2 of 3.
    plans/active/<plan>.md milestone "M2: <name>" did not complete: its
    acceptance command `<command>` exited 1 and no evidence was recorded.
    Progress has been split in place. Read the milestone's Progress entry,
    fix the cause, and re-run; the loop will resume at M2.

    loop-runner: refusing to start.
    plans/active/<plan>.md has a completed Progress entry with no timestamp.
    An unattended run cannot distinguish work it did from work it inherited
    unless the record is current. Update the plan, then re-run.

Both messages name the plan, the milestone or defect, and the resume path. A
runner that halts silently, or that reports only "iteration failed", costs a
human the entire reconstruction the plan file was supposed to make
unnecessary.

## Per-stack hints

The runner is a shell loop around whatever non-interactive entry point a
harness offers, and the harness is the hard part: at least one supported
harness may have none, which limits where the card can be built at all.
Commit-shape assertions come from `git log --name-only` between the
iteration's start and end revisions, and the Progress-entry comparison from
`git show` of the plan file at those two revisions. Keep the fixture plan and
its scratch repository outside this repository, so a self-test failure cannot
leave half-advanced commits in real history.
