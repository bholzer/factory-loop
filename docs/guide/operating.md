# Operating

This page is the runbook for this repository as it is actually run: the
session cadence, the constant prompts that start sessions, the review the
operator supplies between rounds, the watched loop, the commit gate, the
recurring passes, and the ladder. Each practice names the file that owns
its rule; the story of why the loop has this shape is
`docs/guide/overview.md`, and this page assumes it.

## One milestone per session

The working unit is a fresh session against a plan: it reads the plan and
what the plan names, executes the first unfinished milestone, writes what
it observed into that plan's living sections, commits, and stops.
`plans/PLANS.md` owns the rule — its house rules forbid a session from
rolling on into a second milestone — and `skills/plan-execute/SKILL.md`
is the procedure such a session follows. The operator's half is to start
sessions one at a time and read what each left behind, not to steer them
while they run.

## The prompts

Three constant prompts drive nearly everything, recorded here verbatim as
the operator types them — which is why the blocks below are indented
quotations rather than prose. Each one hands the session a file to
follow, so the procedures and the convention stay the entry points and
the prompt itself carries no instruction a file does not own.

To execute the next milestone of a plan in flight:

    Execute the next unfinished milestone of plans/active/<plan>.md,
    following plans/PLANS.md. One milestone, update the living sections,
    commit, stop.

To author a plan:

    Read AGENTS.md first. Then follow skills/plan-author/SKILL.md to author
    a plan for <the goal, naming the debt row or commissioning decision and
    the owner facts research cannot supply>. Stop at a committed plan.

To drive a plan milestone by milestone through the watched loop — a shell
line rather than a session prompt:

    ./tools/loop-runner run <repo-dir> --limit <n> -- <a proven harness
    line from plans/completed/blueprint-live-trial.md>

The angle-bracketed slots are the operator's choices at invocation time.
In the third, the harness invocation comes from a completed plan whose
recorded lines were observed to work; `tools/loop-runner` refuses this
checkout as a target, so the repository directory named there is always
another project's.

## Review between rounds

The review this repository still runs on is a person reading at plan
boundaries. After a plan is authored and before its first milestone runs,
then again between milestones while it is in flight, the operator reads
the diff the last session landed and the plan's living sections beside
it. Findings go into the plan itself, as entries in its Decision Log
dated and carrying the reviewer as author, beside the entries the
sessions wrote. That this is practice, not aspiration, is checkable in
the archive: plans under `plans/completed/` carry reviewer-authored,
dated entries through their Decision Logs.

## The watched loop

`tools/loop-runner` advances a plan in another checkout one fresh session
per milestone, and after each iteration decides from git alone whether to
continue: the commits the iteration left, whether every one touched the
plan being advanced, and which Progress entry changed state. What the
session printed about itself decides nothing — exit codes have been
observed wrong in both directions, and a closing report is the judged
session's own prose —
`docs/decisions/0027-the-loop-runner-is-watched-and-judges-by-commit-shape.md`
holds those observations and the rule they bought. The stop rule itself —
halt on the first iteration that left no observable advance — is the
invariant on `docs/capabilities/loop-runner.md`, and the self-test that
`./tools/verify` runs is what keeps the runner honest about it.

A halt is the tool working, not an alarm. The message names the plan, the
milestone or defect it stopped on, and the way back in — the card holds
the message shapes — so the operator's move is to read the named
milestone's record and the transcript, fix the cause, and start the
runner again; iteration resumes where it stopped. A run that instead ends
at its iteration limit has reached the boundary the operator chose in
advance, which is where the reading above happens. Every run is started,
sized, and read by a person; the same decision record holds why an
unattended loop was refused.

## The gate

Nothing lands here without `./tools/verify` passing: `tools/hooks/pre-commit`
is versioned with the tree, runs that command, and refuses a commit that
fails it, once a clone has pointed git at the hook directory with the
install line `AGENTS.md` publishes:

    git config core.hooksPath tools/hooks

Run that once per clone, first. The caveat is `D8` in `docs/DEBT.md`, and
it belongs in view whenever a green history is being weighed: the hook
can be skipped with a flag that leaves nothing behind, a fresh clone runs
no hook until the line above has been run, and no check yet runs anywhere
the committer cannot reach. The gate is real and it is also, today,
voluntary — which is part of why the ladder below has not moved.

## The recurring passes

Two procedures run on occasions rather than inside milestones, and
noticing the occasion is the operator's job. `skills/doc-garden/SKILL.md`
is the drift sweep. Its own text names when: after a convention or a
knowledge artifact changed while plans were in flight — above all an edit
to `plans/PLANS.md`, which obliges auditing every active plan against the
new text — when a plan has just completed, at a stated cadence measured
in landed changes, and before authoring work that will lean on these
artifacts being current. It runs as its own session, never inside a
milestone's commits, because gardening widens scope.

`skills/retro/SKILL.md` closes work worth learning from: once a plan's
milestones are all done, before the file leaves `plans/active/`, a retro
session compares what the plan intended with what its evidence says
happened, and routes each lesson that survives its materiality test to
exactly one owning file — or explicitly to no change, with the failed
question named. It also runs after one episode expensive enough to stand
by itself. Both passes end in ordinary commits, and the operator reviews
them like any session's.

## The ladder

`docs/MATURITY.md` is the autonomy ladder, and the discipline is that
nobody climbs it by feel. A rung moves up only by that file's promotion
rule — every card gating the rung enforced, green through the stated
stretch of landed changes with nothing skipped or disabled along the way,
and a named human confirming the claim inside the commit that moves the
rung — and it falls at once on the triggers the demotion rule lists, with
the green count starting over. Which rung this repository stands on today
is that file's Current rung section to say, not this page's; the
operator's part is to keep that section honest — claim nothing a check
does not subsume, and record every fall where it happened.
