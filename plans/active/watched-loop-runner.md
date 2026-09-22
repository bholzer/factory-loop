# Build the watched loop runner and retire D5

This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

This document must be maintained in accordance with `plans/PLANS.md`.

## Purpose / Big Picture

Today a human advances a plan by hand: they invoke one milestone session, wait, read what it did, decide whether the record is honest, and only then invoke the next one. That judging loop has now run across four completed plans — `plans/completed/v1-blueprint.md`, `plans/completed/mechanical-gate-set.md`, `plans/completed/blueprint-live-trial.md`, `plans/completed/decide-procedure-location.md` — and the stop rule held every time without human correction, which is exactly the trigger `docs/DEBT.md` row `D5` names for building the machinery.

After this plan, a person can point one command at a git checkout that has a plan in flight — this repository's own checkout is deliberately refused; the first consumer is a project this repository is about to bootstrap — and watch it drive milestone sessions one at a time:

    ./tools/loop-runner run <repo-dir> --limit 2 -- claude -p --model opus --dangerously-skip-permissions

They see each session's output stream live into a log, see the runner refuse to start when the target plan's record is stale, see it halt with a named reason on the first iteration whose milestone did not complete, and see it stop at the iteration limit with a summary naming what advanced. What they can no longer do is what the runner exists to prevent: accidentally run a second milestone on top of an unrecorded first one.

This is the watched runner, not unattended autonomy. A human still chooses the limit, still watches the run, and still reviews at plan boundaries. No maturity rung is claimed, and no row of the Gating capabilities table in `docs/MATURITY.md` changes. The card being built is `docs/capabilities/loop-runner.md`; its status row in `docs/capabilities/index.md` moves from `specced` when the card's own acceptance is observed, and `docs/DEBT.md` `D5` is deleted at close-out.

## Progress

- [x] (2026-09-22 01:59Z) Plan authored: preflight recorded under Artifacts and Notes (verify green in 0.60s, harness version banners), interface and message contracts settled, milestones cut.
- [x] (2026-09-22 02:40Z) M1 — `tools/loop-runner` written and exercised by hand against four scratch fixtures: the passing case (exit 0, two commits each touching the plan, two timestamped ticks, third entry untouched), the split halt (exit 1 after iteration 2), the committed-nothing halt (exit 1 after iteration 1, session exit 0), the stale-record refusal and the dirty-tree refusal (exit 2, stub never invoked), and the inside-this-worktree refusal (exit 2); `AGENTS.md`, `ARCHITECTURE.md`, `docs/specs/mechanical-checks.md` and the covers row in `docs/specs/index.md` name the new command; `./tools/verify` green at 6 of 6; committed as `79dfbc0`.
- [x] (2026-09-22 03:05Z) M2 — `tools/checks/loop-runner` encodes five scenarios (the four hand-run ones plus the unrecorded-advance fixture the sabotage step turned out to require), is last in `tools/verify`'s CHECKS list, and was seen red against the sabotaged runner and green after restoring it; `./tools/verify` reports `7 of 7 checks passed` in 2.13–2.23s against the five-second budget, so the index row reads `enforced` and the budget branch closed on the measurement; the check counts in `docs/specs/mechanical-checks.md`, `docs/specs/index.md`, `docs/MATURITY.md` and the `fast-verify` card's example transcript, the self-test sentence in `ARCHITECTURE.md`, and the status clause in `docs/MATURITY.md` all name the seventh check; committed as `0d379dd`, whose own pre-commit hook ran the new check in the environment that hook supplies and passed.
- [x] (2026-09-22 03:15Z) M3 — Claude Code `2.1.278` drove two milestones of a hand-built smoke checkout under `--limit 2`, watched: exit 0, `2 of 2 iterations advanced`, iterations of 101s and 72s, one commit each touching `plans/active/smoke.md`, both entries ticked and stamped, the third untouched, `one.txt` and `two.txt` holding their words and `three.txt` absent, and no refusal or halt on the way; the smoke checkout had to carry the runner's commit-shape rule in its own `AGENTS.md` for the passing case to be reachable, and both sessions recorded an acceptance carve-out rather than invent a transcript.
- [ ] M4 — close-out: `D5` deleted, `GOALS.md` non-goal reworded, `docs/MATURITY.md` narrative updated, card's Enforcement point rewritten, decision graduated, plan moved to `plans/completed/`.

## Surprises & Discoveries

- Observation: Claude Code moved from `2.1.274` (recorded at trial authoring five days ago) through `2.1.278` (after the operator re-authenticated mid-trial) and still prints `2.1.278 (Claude Code)` today; Codex CLI and omp are unchanged.
  Evidence: `claude --version` → `2.1.278 (Claude Code)`, `codex --version` → `codex-cli 0.150.1`, `omp --version` → `omp/18.1.14`, observed 2026-09-21 while authoring. Harness version drift between authoring and M3 is expected and is why M3 re-records the banner on its day.
- Observation: the verification budget has room for a self-test that builds scratch git repositories.
  Evidence: `./tools/verify` observed 2026-09-21, `fast-verify: 6 of 6 checks passed (0s)`, wall time 0.60 seconds against the five-second budget in `docs/capabilities/fast-verify.md`.
- Observation: a refusal that has already created its log directory makes the operator's retry impossible, because a log directory is never reused.
  Evidence: the first hand-run against the stale-record fixture printed the refusal and left `logsC` behind; the fix creates the directory when the first session is about to run instead, and the same invocation now prints the same refusal while `ls -d` on the log path reports no such file. The dirty-tree refusal was re-run afterwards and leaves nothing either.
- Observation: the loop stops at plan completion before reaching the limit — a path no acceptance clause names, but one the summary had to word.
  Evidence: fixture A with `--limit 3`, after two milestones had already landed, ran a single iteration and printed `1 of 3 iterations advanced …` followed by `The plan has no unticked entries left.`, exit 0.
- Observation: the stub's own artifacts have to live outside the target repository, or the zero-commit fixture stops being one.
  Evidence: the stub writes `brief-received.txt` and `sentinel` to the scratch root rather than into the checkout, so after fixture D's session `git status --porcelain` there is empty and `git log --oneline` shows only the baseline commit — the absent commit is the only trace that session left, which is exactly what the halt reports. The brief log still grew to five entries over the A, B and D runs, which is what proves a session ran at all.
- Observation: the sabotage the plan specified — a runner that keeps iterating when no Progress entry flipped — is invisible to all four authored scenarios, because the only one that reaches the flip judgement (the split fixture) halts anyway on the session's nonzero exit.
  Evidence: with `faults=$((faults + 1))` deleted from the `newdone -eq 0` branch of `tools/loop-runner`, `./tools/verify` reported the advance, split, silent and refusal scenarios all passing and only the fifth, `unrecorded`, failing — seven violations, starting `the run exited 0 where 1 is the only correct answer`, with the sabotaged runner's own transcript claiming `2 of 2 iterations advanced` and naming both entries as `""`.
- Observation: a check that builds git repositories must neutralise git's environment, or it destroys the commit it is gating. The hazard is not theoretical: proving it cost this session its own index and three stray commits.
  Evidence: `env GIT_DIR=$PWD/.git GIT_INDEX_FILE=$PWD/.git/index ./tools/checks/loop-runner` with the `unset` loop disabled — the environment `tools/hooks/pre-commit` really supplies — made every fixture look dirty to the runner (`refusing to start. … has uncommitted changes.`, exit 2 in all five scenarios), and the fixture builder's own `git add -A` and `git commit` wrote into this repository instead: `git log --oneline` then read `Fixture plan in flight` three deep and `git status` reported every real file untracked. Recovered with `git reset --mixed 5828ec0`, which restored HEAD and the index and left the working tree's edits untouched. With the loop in place the same invocation passes all five scenarios.
- Observation: the seventh check costs about four times the other six together, and the budget still holds.
  Evidence: `./tools/verify` timed three times at 2.23s, 2.13s and 2.16s against 0.55s before, with `tools/checks/loop-runner` alone at 1.74–1.90s; the five-second figure in `docs/capabilities/fast-verify.md` is unchanged and unthreatened, but the order-of-magnitude margin that spec claimed is gone and the sentence claiming it was rewritten.
- Observation: one broken scenario produces seven violations that share one cause, and seven copies of the remediation bury the seven facts.
  Evidence: the first sabotage run printed the same four-line remediation paragraph after every fault; `fault` now prints a remediation only when it differs from the previous one, which is the rule `tools/blueprint-eval`'s fill part already applies per file.
- Observation: the runner's commit-shape rule and the commit cadence `skills/plan-execute/SKILL.md` asks for pull in opposite directions, and a driven project has to settle that in its own record before a run can pass.
  Evidence: a session that commits its work and then its plan update leaves one commit touching nothing in the plan, which is the `commit <sha> changed nothing in <plan>` fault. The smoke checkout's `AGENTS.md` therefore carries a working rule fixing one commit per milestone that holds both, and the smoke plan's first decision gives the reason; both driven sessions obeyed it without remark, and the second cited it when declining to amend. Nothing this repository ships says it yet.
- Observation: a real harness handed an acceptance clause it cannot satisfy reports the shortfall instead of inventing the output — the behavior the whole judging design assumes, observed rather than hoped.
  Evidence: the smoke plan asks each milestone for `git show --stat HEAD` of the very commit carrying its record, which cannot be transcribed into that commit. Both sessions ran the command, reported it in their closing message, and wrote the carve-out into the smoke plan's Surprises and Decision Log. The runner's judgement was untouched: a carve-out is prose, and the entry each session ticked was still one, stamped, and inside a commit that changed the plan.
- Observation: what a halt message calls a transcript is, for this harness, one closing report.
  Evidence: the two iteration logs are 1267 and 1933 bytes for sessions that ran 101 and 72 seconds, because `claude -p` streams nothing until its final message. The committed-nothing halt tells an operator that the last lines of the log say what the harness did instead; here the last lines are the whole file, which is enough to read but is not a record of the session's work.
- Observation: a watched run of two trivial milestones costs about ninety times what the self-test gating it costs.
  Evidence: 173 seconds wall for `--limit 2`, iterations of 101s and 72s, against 1.74–1.90s for `tools/checks/loop-runner` over five fixtures. The gap is the whole reason the runner is not one of the checks.

## Decision Log

- Decision: build the watched runner now; treat `D5`'s trigger as fired.
  Rationale: the trigger reads "enough consecutive sessions whose stop rule held without human correction that the halt conditions are known". The owner attests, in the conversation that commissioned this plan, that the stop rule held across the four completed plans, and the halt conditions an automated judge must recognize are recorded as refusal evidence in `plans/completed/blueprint-live-trial.md` and `plans/completed/decide-procedure-location.md` (catalogued under Context and Orientation below).
  Date/Author: 2026-09-21 / plan-author session.

- Decision: rewording the `GOALS.md` non-goal ("No background automation in v1…") is in scope, as an agreed scope change, and lands in M4.
  Rationale: `skills/plan-author/SKILL.md` forbids smuggling scope changes into milestones; this one was agreed explicitly by the owner when commissioning the work — scope is the watched runner with an iteration limit, halt-on-anything-unexpected, and a human reviewing at plan boundaries; unattended operation and recurring GC agents stay out, and no rung is claimed. `GOALS.md` edits are human-approved at L0, which the plan-boundary review satisfies.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: the runner is harness-agnostic — it takes the session command as trailing arguments after `--` and appends the brief as one final argument, rather than hard-coding the three harnesses.
  Rationale: the card says the runner is "a shell loop around whatever non-interactive entry point a harness offers". Parameterizing keeps the loop identical across Claude Code, Codex CLI, and omp (whose proven invocation lines differ only in argv), makes the self-test possible with a stub session command and no network, and means the next harness costs zero runner edits.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: an iteration is judged by commit shape and plan diff, never by exit status alone; a nonzero session exit halts the run even when the commit shape looks clean.
  Rationale: recorded refusal evidence shows exit status is untrustworthy in both directions — Claude Code exited 0 on a credits refusal having committed nothing (`plans/completed/blueprint-live-trial.md`, M2 surprises), while Codex CLI exits nonzero for account, model, and directory refusals. The card's invariant is stated over commits and Progress entries, so that is what the runner reads, via `git log --name-only` and `git show <rev>:<plan>` at the iteration's start and end revisions. The extra halt on nonzero exit is the owner's halt-on-anything-unexpected rule: a session that ticked a milestone and then died is a session a human should read before iteration continues.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: the runner refuses to drive the checkout it runs from; targets inside this repository's worktree exit 2 before any session starts.
  Rationale: same reason `tools/blueprint-eval` refuses trials inside the worktree, extended from the card's fixture rule ("keep the fixture plan and its scratch repository outside this repository"): a runaway session editing the runner's own checkout mutates the tool mid-run and can leave half-advanced commits in real history. The first consumer is another checkout anyway.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: the runner's precondition reimplements the structural contract of `docs/capabilities/evidence-check.md` over the target's plan file instead of invoking `tools/checks/evidence-check`.
  Rationale: the card says the runner "defers to the structural contract specified in `docs/capabilities/evidence-check.md`" — the specification, not the executable. The executable is bound to this repository's `plans/active/` by `git rev-parse --show-toplevel` and checks a whole directory; the runner judges one named file in a foreign checkout. Precedent: `tools/blueprint-eval` deliberately reimplements the reference definition of `docs/capabilities/doc-integrity.md` for the same reason, and says so in its header.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: the per-iteration brief is the trial's proven execution brief with one change — it names the plan file explicitly.
  Rationale: brief three of `plans/completed/blueprint-live-trial.md` drove execution sessions in all three harnesses; proven wording is kept. The addition ("a plan in flight at `<plan>`") exists because the runner has already resolved which plan it will judge, and a target with two plans in flight must not let the session pick the other one.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: halt messages report what the runner observed — session exit status, commit count, which Progress entry changed and how — not the milestone's internal acceptance command.
  Rationale: the card's example message names "its acceptance command `<command>`", but only the session can see inside a milestone; the runner sees revisions. The card's stated bar is that both messages "name the plan, the milestone or defect, and the resume path", which the observed-facts wording meets without inventing knowledge the runner does not have.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: the card's Invariant section keeps its "unattended run" wording; only its Enforcement point paragraph is rewritten at close-out.
  Rationale: the invariant describes the run's behavior — one milestone per iteration, halt on the first unobserved acceptance — which is identical watched or unattended; what v1 withholds is running it with nobody watching, and that boundary belongs to `GOALS.md` and `docs/MATURITY.md`, not to the invariant. Rewriting a spec the build just satisfied would also orphan the acceptance evidence recorded against its exact text.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: the self-test lives in `tools/checks/loop-runner` and is appended last in `tools/verify`'s CHECKS list; if its measured cost busts the five-second budget it is left out of CHECKS and the card stops at `built` instead of `enforced`.
  Rationale: one check per card is the check-layer convention in `ARCHITECTURE.md`, and this repository's substitute for the card's "continuous integration alongside the other checks" is `./tools/verify` plus the pre-commit hook — nothing runs on the remote (`docs/DEBT.md` `D8`). Last in order because it is the slowest. The budget branch is decided by measurement in M2, not by hope; both outcomes leave the index row honest.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: M3's smoke checkout is built by hand from `plans/PLANS.md`, `skills/`, and a trivial plan, not by `./tools/blueprint-eval new`.
  Rationale: a fresh trial copy is an unfilled payload with no plan in flight; making it drivable costs a driven bootstrap and a driven authoring session (about twenty minutes of harness time) and would test the payload again rather than the runner. The hand-built checkout carries exactly what a bootstrapped project has that the runner and its sessions touch: the convention, the procedures, and a conforming plan.
  Date/Author: 2026-09-21 / plan-author session.

- Decision: (pre-execution review) a fourth fixture, D, exercises the
  zero-commit halt — the stub is invoked, appends the brief, creates
  nothing, commits nothing, and exits 0 — and M1's hand-runs and M2's
  self-test both carry it.
  Rationale: the halt on "no new commits regardless of exit status" is the
  design's motivating case — the exit-zero credits refusal heads this plan's
  own halt catalogue — and its message shape is drafted under Interfaces and
  Dependencies, yet none of the three scenarios produces it: A passes, B
  splits with exit 1, C never invokes a session. A published message nobody
  has seen print is a message that has never been checked, and this one
  guards the failure most likely in real use.
  Date/Author: 2026-09-22, reviewer.

- Decision: an entry's identity across two revisions is its text normalised — lowercased, with any parenthesised date and all punctuation stripped — and never its position in the list; a flip is a key complete at the end revision that was not complete at the start.
  Rationale: a session that rewords an entry while ticking it is doing what `plans/PLANS.md` asks (the split shape "completed: X; remaining: Y" is the convention's own), and position-based matching turns every inserted or split entry into a false halt. Stripping the date is what lets the ticked form of an entry match its own unticked form one revision earlier.
  Date/Author: 2026-09-22 / M1 session.

- Decision: the log directory is created when the first session is about to run, not when the run starts.
  Rationale: observed during M1's hand-runs, recorded above. A refusal is the operator's cue to fix the target and re-issue the same command; a directory left behind by the refusal makes that second command refuse for a reason the operator never caused.
  Date/Author: 2026-09-22 / M1 session.

- Decision: two refusal vocabularies — `loop-runner: cannot run — …` when the invocation itself is impossible (usage, a target that is not a git worktree, a target inside this worktree, a log directory that already exists), and `loop-runner: refusing to start.` when the invocation is fine and the target's record is not (uncommitted changes, an unresolvable plan, a stale record).
  Rationale: the card's message contract fixes the second shape's wording and `tools/blueprint-eval` fixes the first; keeping them distinct tells the reader at a glance whether to edit the command line or the plan.
  Date/Author: 2026-09-22 / M1 session.

- Decision: the covers row for `mechanical-checks.md` in `docs/specs/index.md` was edited in M1, though the plan assigned that file to M2.
  Rationale: the row said two commands are what a reader can run, which the third command makes false in the commit that lands it; the check count in the same row still reads six and stays M2's to change. Shipping a commit whose own index contradicts it costs more than touching one clause early.
  Date/Author: 2026-09-22 / M1 session.

- Decision: the fixture stub writes its brief log and its sentinel outside the target repository.
  Rationale: recorded above as an observation. It also keeps M2's check able to assert both halves of the silent-refusal scenario — a session was invoked, and the checkout it ran in is untouched — without the stub's own bookkeeping being the thing that dirties the tree.
  Date/Author: 2026-09-22 / M1 session.

- Decision: a fifth fixture, `unrecorded`, joins the four: a session that commits to the plan, exits 0, and ticks nothing. M2's acceptance clause therefore reads five scenario lines rather than four.
  Rationale: the sabotage this milestone owes the promotion bar is "continue iterating when no Progress entry flipped", and none of the four authored fixtures can see it — the split fixture is the only one reaching that judgement and it halts on the nonzero exit regardless. A sabotage no scenario catches is not a demonstration, and the clause it attacks is the one that matters most in real use: a session that did the work and left the record stale. Recorded above with the observation that produced it.
  Date/Author: 2026-09-22 / M2 session.

- Decision: the check unsets every `GIT_*` variable it inherits and exports its own author and committer identity before building a fixture.
  Rationale: `tools/hooks/pre-commit` runs `./tools/verify` with `GIT_DIR` and `GIT_INDEX_FILE` pointing at this repository, so a fixture's `git add -A` inherits them and stages into the commit being gated — observed above, at the cost of this session's index and three stray commits. The identity is exported for the same hermeticity reason: a machine whose git has no `user.email` would otherwise turn this check into an undecidable one, and the fixtures are throwaway repositories whose authorship nobody reads. Fixture commits also pass `--no-verify`, so a globally configured hooks path cannot recurse into this repository's own gate.
  Date/Author: 2026-09-22 / M2 session.

- Decision: the check's five scenarios pin four clauses of the invariant and deliberately leave four guards unpinned — two entries completed in one iteration, an entry completed without a timestamp, an extra commit touching nothing in the plan, and a completed entry un-completed.
  Rationale: each of those is one more `faults` increment beside the ones the fixtures do exercise, in the same judging block of `tools/loop-runner`, so the marginal fixture buys less than the five already do; and every added scenario costs wall time inside a budget the seventh check already spends four fifths of. The boundary is written into the check's own header rather than left implicit, so the next session extending it knows what is uncovered rather than assuming coverage.
  Date/Author: 2026-09-22 / M2 session.

- Decision: the status clause in `docs/MATURITY.md`'s Current rung, and the check counts in `docs/specs/mechanical-checks.md`, `docs/specs/index.md`, `docs/MATURITY.md` and the `fast-verify` card's example transcript, were all corrected in M2 although M4 owns the maturity narrative.
  Rationale: the same reason M1 touched the covers row early — the sentence "Of the L2 rows … `loop-runner` gates a rung two steps away" reads as a contrast with the enforced rows, which the index row landing in this commit makes false. M4 keeps that sentence for the watched-boundary wording that arrives with the `GOALS.md` rewrite; what M2 corrected is only the status contradiction and the arithmetic. The spec's claim that the checks stay "an order of magnitude inside the published figure" was rewritten for the same reason: at 2.2 seconds against five it is no longer true.
  Date/Author: 2026-09-22 / M2 session.
- Decision: the smoke checkout states the commit shape the runner judges, in its own `AGENTS.md` working rules, and its plan records why.
  Rationale: recorded above as an observation. The alternative was to let the first session split work and record across two commits, watch the run halt on the untouching commit, and offer that as the milestone's evidence — but an environment-shaped halt is not the passing observation M3 requires, and this one would have been caused by the fixture rather than found by it. A bootstrapped project driven by this runner has to carry the rule anyway; learning that it must is part of what a rehearsal is for.
  Date/Author: 2026-09-22 / M3 session.

- Decision: per-iteration wall times are derived from the run's start epoch and each iteration log's modification time rather than measured inside the runner.
  Rationale: the runner prints no timings, and adding them is a contract change this milestone has no mandate for. `tee` writes the log as the session streams, so its mtime is that iteration's end to within a second, which is the precision a "how long does a watched run take" claim needs.
  Date/Author: 2026-09-22 / M3 session.

- Decision: the smoke checkout and its log directory are left in place under `$HOME/blueprint-trials/` rather than deleted at the end of the milestone.
  Rationale: they are what the evidence below points at, and the same place `tools/blueprint-eval` leaves its trial repositories, so an operator inspecting either knows one location. M2's scratch was deleted because a check that leaves directories behind fails its own hygiene assertion; a hand-run rehearsal has no such constraint and its artifact is worth more kept than tidy.
  Date/Author: 2026-09-22 / M3 session.

## Outcomes & Retrospective

M1, 2026-09-22. The runner exists and does the thing the card describes: pointed at a scratch checkout with a three-milestone plan and a stub harness, it advanced two milestones, left one commit per iteration touching the plan, ticked each entry with a timestamp, left the third alone, and printed a summary naming all of it. The three failure classes the card requires were each produced rather than argued: a milestone that could not complete halted the run with the entry text and the log path, a session that exited 0 having committed nothing halted it on the absent commit, and a plan carrying an undated completed entry was refused before any session started — proven by a sentinel the stub touches on every invocation, which stayed absent. Two behaviors the card does not name were settled by running them: the loop stops early when the plan runs out of unticked entries, and a refusal now leaves no log directory behind, so the operator's retry can reuse the same path.

What remains for the rest of the plan is unchanged: the stop rule is exercised only by hand, so nothing re-checks it on a later edit (M2), no real harness has been driven through it (M3), and every document that still describes a world without a runner — `GOALS.md`, `docs/MATURITY.md`, the card's Enforcement point, `docs/DEBT.md` row `D5` — is M4's. The one lesson worth carrying: the fixtures are what made the design decidable, and writing them before the runner would have been cheaper than writing them alongside it, because each halt message got its wording from watching the fixture that produces it.

M2, 2026-09-22. The stop rule is now decided by a machine on every commit to this repository: `tools/checks/loop-runner` builds five throwaway repositories under `mktemp -d`, drives `./tools/loop-runner` over each with a stub harness that does exactly what the fixture's next entry says, and asserts the exit status, the message fragments the card's contract fixes, the commits each fixture gained, which of them touched the plan, the checkbox and timestamp shapes left behind, the files created or absent, and the transcripts the sessions streamed into. It runs last in `./tools/verify`, which now reports seven of seven in about 2.2 seconds against a five-second budget, so the card reads `enforced` rather than `built` and the budget branch this plan carried closed on a measurement instead of a hope.

Two things came out of the session that the plan did not anticipate. The sabotage the plan specified could not be caught by the scenarios the plan specified, which is why a fifth fixture exists and why the promotion bar is worth paying rather than asserting: the demonstration found a hole in the demonstration. And the check's own hazard — a self-test that builds git repositories while running inside a pre-commit hook — bit this session for real when it was deliberately exercised against this checkout, clobbering the index and landing three fixture commits on the branch before `git reset --mixed` put it back. Both are recorded above with their evidence.

What remains: no real harness has been driven through the runner yet (M3), and every document that still describes a world without one — `GOALS.md`, the card's Enforcement point, the rung narrative's watched-boundary wording, `docs/DEBT.md` row `D5` — is M4's.

M3, 2026-09-22. The runner drove a real harness through two milestones of a real plan in a checkout that has never heard of this repository, and the claim at the head of this plan — point one command at a checkout with a plan in flight and watch it advance one milestone at a time — is now an observation. The smoke checkout was built by hand as the milestone specified: `git init`, this repository's `plans/PLANS.md` and `skills/` copied in, a nine-line `AGENTS.md` that names the convention, the plan and the procedure and says outright that no verification command exists here, and a three-milestone plan whose milestones each create one named file. Claude Code `2.1.278` executed M1 and M2 of it under `--limit 2`, exit 0, 173 seconds, one commit per iteration, each touching the plan, each ticking exactly one entry with a stamp, leaving the third alone. Nothing about the runner needed changing to make that happen.

Two things the rehearsal taught that the fixtures could not. The first is a gap in what this repository ships: the runner requires every commit of an iteration to touch the plan, while the execution procedure tells a session to commit at each coherent step, and a session doing the obvious thing — work first, record second — halts a run on its second commit. The smoke checkout was written with the rule in its `AGENTS.md`, which is what made the passing case reachable, and that rule now exists only in a scratch repository outside this tree. The second is reassurance rather than a gap: handed an acceptance clause that cannot be satisfied inside the commit it describes, both sessions named the shortfall in the plan and reported the command out of band, which is exactly the honesty the commit-shape judgement assumes and cannot itself verify.

What remains is M4 alone: `docs/DEBT.md` row `D5`, the `GOALS.md` non-goal, the card's Enforcement point, the rung narrative's watched-boundary wording, decision 0027, and moving this file to `plans/completed/`.

## Context and Orientation

This repository is a blueprint for agent-operated projects. `template/` is a payload copied into new projects; the repository root is the same payload filled in for this project; `skills/<name>/SKILL.md` are portable procedures; `tools/` holds this project's own mechanical checks (`tools/checks/*`, aggregated by `./tools/verify`, enforced by `tools/hooks/pre-commit`) plus `tools/blueprint-eval`, a driver that builds and inspects trial repositories outside this tree. Multi-session work is governed by `plans/PLANS.md`: a plan is a self-contained file, a session executes exactly one milestone of it and writes observed evidence back into its living sections, and the loop that advances milestones lives outside the session. Until now that loop has been a human.

`docs/capabilities/loop-runner.md` is the specification this plan builds, written before any runner existed. Its invariant, restated: each iteration produces at least one commit and every such commit touches the plan file it is advancing; across one iteration exactly one Progress entry changes state, and a completion carries a timestamp; no iteration begins while the previous milestone's acceptance is unrecorded; and the run halts on the first iteration whose milestone it could not complete, leaving Progress split in place. Its acceptance names one passing case (a three-milestone fixture plan in a scratch repository, an iteration limit of two, exactly two advanced) and two required failing cases (halt with a named reason when the second milestone cannot complete; refuse to start on a stale record). The "stale record" contract it defers to is `docs/capabilities/evidence-check.md`: the four living sections present with text under each, and every ticked Progress entry carrying a parenthesised `YYYY-MM-DD` timestamp.

Timestamped checkbox entry means exactly what `tools/checks/evidence-check` decides: a line matching `- [x]` whose text contains `(YYYY-MM-DD`, in the Progress section, outside fenced or indented blocks. The runner reads the same shapes: unticked entries are `- [ ]` lines, and a "flip" is an entry whose checkbox changed from unticked to ticked between two revisions of the plan file.

The halt conditions the runner must recognize are not hypothetical; they were each observed and recorded while humans ran the loop by hand:

- A session that refuses and exits 0 having committed nothing. Claude Code printed a two-line credits refusal, exited 0, and made no commit (`plans/completed/blueprint-live-trial.md`, Surprises, M2). This is why zero new commits is a halt regardless of exit status.
- A session refused by account state, exiting 1: Claude Code's "You've hit your session limit · resets 8:20pm" (`plans/completed/decide-procedure-location.md`, Surprises); Codex CLI's usage-limit and model-version refusals (`plans/completed/blueprint-live-trial.md`, Surprises, M5).
- Refusals before any model call: Codex CLI exits 2 on mutually exclusive flags and exits 1 outside a git repository. And its first output line, "Reading additional input from stdin...", appears even with stdin closed and means nothing — the runner must not parse harness chatter, only commits and the plan.

The proven non-interactive invocation lines, one per harness, each recorded with observed runs in `plans/completed/blueprint-live-trial.md` (banners re-observed 2026-09-21: `2.1.278 (Claude Code)`, `codex-cli 0.150.1`, `omp/18.1.14`); the runner supplies the working directory, closed stdin, and the trailing brief argument:

    claude -p --model opus --dangerously-skip-permissions
    omp -p --auto-approve
    codex exec -m gpt-5.6-sol --approve-for-me

Files this plan touches, and where the close-out edits land: `tools/loop-runner` (new), `tools/checks/loop-runner` (new), `tools/verify` (CHECKS list), `AGENTS.md` (Commands section currently says "Two commands exist"), `ARCHITECTURE.md` (Check layer component enumerates what lives in `tools/`), `docs/specs/mechanical-checks.md` and `docs/specs/index.md` (say "two commands" and "six checks"), `docs/capabilities/index.md` (the `loop-runner` row reads `specced` / `—`), `docs/capabilities/loop-runner.md` (Enforcement point says "Nothing enforces it today"), `docs/MATURITY.md` (Current rung narrative says `loop-runner` "gates a rung two steps away"; the Gating capabilities table is not touched), `GOALS.md` (Non-goals line 66), `docs/DEBT.md` (row `D5`), and `docs/decisions/0027-…` (new record; 0026 is spent).

Constraints binding every session here: `./tools/verify` must stay green at every commit (the hook runs it); `doc-integrity` requires any path named in a live artifact to exist in the same commit, so a doc mention of a new file lands with the file; `prose-duplication` compares eight-word windows across artifacts, so restated facts must be reworded and quoted material indented; at L0 a human approves every edit to `GOALS.md` and every capability status change, which plan-boundary review provides.

## Milestones

### M1 — The runner, observed by hand

Scope: write `tools/loop-runner` implementing the interface, preconditions, iteration judgement, and messages specified under Interfaces and Dependencies below; make the three live documents that enumerate commands tell the truth about it; prove all three behavior classes by hand against scratch fixtures using a stub session command, before any self-test exists. At the end of this milestone a person can run the passing case from the card's Acceptance section themselves.

The work: create the runner as POSIX shell beside `tools/blueprint-eval`, following that file's conventions (header comment naming the card, `cd "$(git rev-parse --show-toplevel)"`, `LC_ALL=C`, exit 0/1/2, refusals that name the next action). Build fixtures A, B, and C (shapes under Interfaces and Dependencies) in a directory from `mktemp -d`, with the stub session script beside them. Update `AGENTS.md`'s Commands section (two commands become three; stay inside the ~100-line cap), the Check layer component of `ARCHITECTURE.md` (the runner sits beside `tools/blueprint-eval`, is not a check, and `tools/verify` does not run it — yet), and the "What a reader can run" opening of `docs/specs/mechanical-checks.md`. Commit the runner and the document edits together so `doc-integrity` resolves.

Acceptance, each run and its output recorded here:

1. `./tools/loop-runner` with no arguments prints usage and exits 2; `./tools/loop-runner run <fixture-A> --limit 2` with no `--` session command refuses with a message naming what is missing and exits 2.
2. Passing case: against fixture A with the stub, `--limit 2` exits 0; the summary names both advanced entries, their commits, and the log directory; `git -C <fixture-A> log --name-only` shows exactly two new commits, each touching `plans/active/fixture.md`; both newly ticked entries carry timestamps; the third entry is untouched.
3. Halting case: against fixture B with the stub, `--limit 3` exits 1 after the second iteration; the message names the plan, the milestone entry text, what was not observed, and the resume path; the third entry was never started; Progress shows the split.
4. Silent-refusal case: against fixture D with the stub, `--limit 2` exits 1 after the first iteration with the committed-nothing message — the session exited 0 and produced no commit — pointing at the iteration log; no Progress entry changed and no commit was made.
5. Refusing case: against fixture C, the runner exits 2 naming the plan and the defect (a completed entry with no timestamp), and the stub's sentinel file proves no session was invoked. Then make fixture A dirty with an untracked `stray` file, re-run the passing invocation, observe the dirty-tree refusal, and delete the stray file.
6. A target inside this worktree is refused: `./tools/loop-runner run . --limit 1 -- true` exits 2 with the inside-worktree message.
7. `./tools/verify` prints `6 of 6 checks passed` and `git status --porcelain` shows only intended files.

### M2 — The self-test, the wiring, and the status

Scope: encode M1's four hand-run scenarios as `tools/checks/loop-runner`, wire it into `./tools/verify`, measure the budget, demonstrate the check failing against a sabotaged runner — the promotion bar in `docs/capabilities/CARD_FORMAT.md` — and move the index row. At the end of this milestone the stop rule is checked on every commit to this repository, or the plan records the measured reason it is not. Executed with one addition: a fifth fixture, `unrecorded`, without which the specified sabotage is invisible (Decision Log, 2026-09-22 / M2 session).

The work: the check builds fixtures A, B, C, D and E and the stub in `mktemp -d` scratch (never inside the worktree), runs `./tools/loop-runner` against each, and asserts the observable facts from M1's acceptance: exit codes, the required message fragments, commit counts and touched paths, flip and timestamp shapes, the untouched third entry, the zero-commit halt on the silent fixture, the unticked-record halt on the unrecorded fixture, and the uninvoked-stub sentinel in the refusing case. One report line per scenario, a summary line, exit 0/1/2, scratch removed on every path. Append `loop-runner` to the CHECKS list in `tools/verify`. Update the check counts in `docs/specs/mechanical-checks.md` (six become seven, plus a paragraph on what this check decides) and the covers column of `docs/specs/index.md`. Move the `loop-runner` row in `docs/capabilities/index.md` to `enforced` at `./tools/verify`, `tools/hooks/pre-commit`.

The sabotage demonstration, in this order, committing none of it: edit `tools/loop-runner` to continue iterating when no Progress entry flipped (the exact edit and diff go in this plan); run `./tools/verify`; observe the loop-runner check fail naming the violated behavior; restore the runner; observe verify green. A self-test never seen red proves nothing, which is the same bar every other card here paid.

Budget branch: record the observed wall time of `./tools/verify` with the check wired in. Inside five seconds, the row reads `enforced` and this branch closes. Over five seconds, remove `loop-runner` from CHECKS, record the measurement, set the row to `built` with `Enforced at` reading `./tools/checks/loop-runner` as the on-demand command, and M4 words the card's Enforcement point accordingly — `docs/capabilities/fast-verify.md` makes the budget part of its invariant, and busting one card to promote another is not available.

Acceptance:

1. `./tools/checks/loop-runner` alone on a clean tree prints five scenario lines and an ok summary, exits 0, and leaves nothing behind in `$TMPDIR`. Observed 2026-09-22: five lines, `ok — 5 scenarios driven through ./tools/loop-runner, 6 stub sessions invoked, every assertion held`, exit 0, and the `mktemp -d` directory count in `$TMPDIR` unchanged across the run (40 before, 40 after).
2. `./tools/verify` prints `7 of 7 checks passed (Ns)` with the observed N recorded here and judged against the budget (or the `built` branch is recorded with its measurement). Observed: `7 of 7 checks passed (2s)`, wall 2.23s, 2.13s, 2.16s over three consecutive runs — inside the five-second budget, so the `enforced` branch is the one taken.
3. The sabotaged-runner run: verify exits 1 with the check's message visible; the restore run: verify green. Both transcripts in this plan. Observed under Artifacts and Notes.
4. `docs/capabilities/index.md` shows the new status; the L0 human approves it at review.

### M3 — A real harness drives a real plan, watched

Scope: the first consumer rehearsal. Build a smoke checkout outside this tree shaped like a bootstrapped project, then watch a real harness advance two milestones of a real plan under the runner. At the end of this milestone the claim "the runner can drive a plan in another checkout" is an observation, not a design.

The work: create `$HOME/blueprint-trials/loop-smoke-<yyyymmdd-hhmmss>/repo` by hand — `git init`, copy this checkout's `plans/PLANS.md` and `skills/` in (they are the portable halves a bootstrapped project carries), write a minimal `AGENTS.md` (a map naming the convention and the plan; no verification command exists there and it must not claim one), and write `plans/active/smoke.md`: three milestones of the fixture-A shape (create a named file with named content; acceptance is reading it back), living sections initialized, conforming to `plans/PLANS.md`. Commit as the fixture's first commit. Re-record the harness banner that day. Then run, watched:

    ./tools/loop-runner run $HOME/blueprint-trials/loop-smoke-<stamp>/repo --limit 2 -- claude -p --model opus --dangerously-skip-permissions

If the environment refuses Claude Code that day (the session-limit and credits refusals above), fall back to `omp -p --auto-approve`, then `codex exec -m gpt-5.6-sol --approve-for-me`, recording each refusal transcript — an environment refusal under the runner is itself an observation of the halt path and is recorded as one, but it does not substitute for the passing observation, which this milestone must obtain from whichever harness runs.

Acceptance:

1. The exact invocation, harness banner, and per-iteration wall times recorded here; the runner exits 0 with its summary naming two advanced milestones and the log directory.
2. In the smoke repo: `git log --name-only` shows each iteration's commits touching `plans/active/smoke.md`; both ticked entries are timestamped; the third is untouched; the created files hold their named content.
3. Any refusal or halt encountered en route is recorded with the runner's own message and the log tail it pointed at.
4. This repository's tree is untouched except this plan file: `git status --porcelain` here shows only `plans/active/watched-loop-runner.md`.

### M4 — Close-out

Scope: make every document that described the pre-runner world true again, retire the debt row whose trigger fired, graduate the durable decision, and close the plan. No gating table row changes and no rung is claimed.

The work, every edit already drafted under Interfaces and Dependencies: delete row `D5` from `docs/DEBT.md` (it has no Details subsection to remove); replace the `GOALS.md` non-goal line; update the one Current-rung sentence in `docs/MATURITY.md` that places `loop-runner` among the L2 rows, to match the index row M2 landed; rewrite the second paragraph of the card's Enforcement point section, which currently reads "Nothing enforces it today", to name the self-test, its enforcement, and the v1 watched boundary `GOALS.md` now records; write `docs/decisions/0027-the-loop-runner-is-watched-and-judges-by-commit-shape.md` per `docs/decisions/DECISION_FORMAT.md`, graduating this plan's two durable decisions (watched scope without a rung claim; commit-shape judgement over exit status); write Outcomes & Retrospective; move this file to `plans/completed/watched-loop-runner.md`.

Acceptance:

1. `grep -n 'D5' docs/DEBT.md` prints nothing; the register table still renders with its remaining rows.
2. `grep -rn 'Nothing enforces it today' docs/capabilities/loop-runner.md` prints nothing; `GOALS.md` no longer says `loop-runner` is "specced and parked"; the `docs/MATURITY.md` narrative agrees with `docs/capabilities/index.md`; the Gating capabilities table is byte-identical to before this plan.
3. `docs/decisions/0027-the-loop-runner-is-watched-and-judges-by-commit-shape.md` exists and follows the format.
4. `./tools/verify` green; the plan file is under `plans/completed/` and `plans/active/` is empty; the human approves the `GOALS.md` and `MATURITY.md` edits at review.

## Plan of Work

M1 writes one new executable, `tools/loop-runner`, modeled line-for-line on the conventions of `tools/blueprint-eval` (header comment citing the card, toplevel `cd`, `LC_ALL=C`, `usage()`, refusal messages that state the next action, exit 0/1/2). Its `run` subcommand parses `<repo-dir>`, optional `--plan`, required `--limit`, optional `--logs`, and the session argv after `--`; refuses targets inside this worktree, dirty targets, unresolvable plans, and stale records; then loops: snapshot `HEAD`, run the session from the target directory with stdin closed and output teed to a numbered log, snapshot again, judge, and either continue, stop at the limit or plan completion with a summary, or halt with a message. The judging reads `git log --name-only <start>..<end>` for commit shape and diffs the checkbox lines of `git show <start>:<plan>` against `git show <end>:<plan>` for flips, reversions, timestamps, and splits. M1 also lands the three document edits that keep `AGENTS.md`, `ARCHITECTURE.md`, and `docs/specs/mechanical-checks.md` honest, in the same commit as the tool.

M2 writes `tools/checks/loop-runner`, which is the fixture harness from M1's hand-runs made permanent: build scratch, run the runner three ways with the stub, assert the facts, report, clean up. It edits one line of `tools/verify` (CHECKS gains `loop-runner` at the end), the two spec files' counts, and the index row — then performs the sabotage demonstration without committing the sabotage.

M3 builds the smoke checkout by hand, runs the runner against it with a real harness, and writes the evidence back here. It edits nothing in this repository except this plan.

M4 makes the five close-out document edits drafted below, writes decision 0027, and moves this file. Each milestone commits frequently and leaves `./tools/verify` green; nothing in this plan touches `template/`, because the payload ships no loop-runner card and `tools/` is live-only by the layer map.

## Concrete Steps

Observed while authoring, 2026-09-21, from the repository root:

    ./tools/verify
    # observed: six per-check report blocks, then
    # fast-verify: 6 of 6 checks passed (0s).  — wall time 0.60s, exit 0

    claude --version ; codex --version ; omp --version
    # observed: 2.1.278 (Claude Code) / codex-cli 0.150.1 / omp/18.1.14

    git status --porcelain | wc -l
    # observed: 0 before this plan file was created

Observed during M1, 2026-09-22, from the repository root. Scratch is `$T` from `mktemp -d`; fixtures A, B, C and D and the stub were rebuilt there from their first commits so that every line below ran against the committed runner:

    ./tools/loop-runner
    # observed: the usage line, exit 2

    ./tools/loop-runner run "$T/A" --limit 2
    # observed: "cannot run — no session command: everything after -- is the
    # harness invocation each iteration runs.", exit 2

    ./tools/loop-runner run "$T/A" --limit 2 --logs "$T/logsA" -- "$T/stub-session"
    # observed: exit 0; two iterations; "2 of 2 iterations advanced
    # plans/active/fixture.md", both entries named with commits 132243f and
    # ab1dce9 and their logs; "1 Progress entry remains unticked."

    git -C "$T/A" log --name-only --oneline
    # observed: two new commits above the baseline, each listing
    # plans/active/fixture.md; one.txt and two.txt hold their words, three.txt
    # does not exist, both ticked entries carry (2026-09-22 02:36Z)

    ./tools/loop-runner run "$T/B" --limit 3 --logs "$T/logsB" -- "$T/stub-session"
    # observed: exit 1 after iteration 2; the halt names the plan, the M2
    # entry text, "the session exited 1 and left 1 commit; no Progress entry
    # became complete", the log, and the resume path; M3 untouched

    ./tools/loop-runner run "$T/D" --limit 2 --logs "$T/logsD" -- "$T/stub-session"
    # observed: exit 1 after iteration 1; "did not advance: the session exited
    # 0 and produced no commit."; fixture D still at its baseline commit with
    # its Progress unchanged

    rm -f "$T/sentinel"
    ./tools/loop-runner run "$T/C" --limit 1 --logs "$T/logsC" -- "$T/stub-session"
    # observed: exit 2; "has a completed Progress entry with no timestamp at
    # line 8"; $T/sentinel absent, so no session was invoked, and $T/logsC was
    # never created

    : > "$T/A/stray"
    ./tools/loop-runner run "$T/A" --limit 2 --logs "$T/logsA2" -- "$T/stub-session"
    # observed: exit 2; "has uncommitted changes."; the stray file was deleted
    # afterwards and fixture A is clean again

    ./tools/loop-runner run . --limit 1 -- true
    # observed: exit 2; "is inside this repository's worktree."

    ./tools/verify ; git status --porcelain
    # observed: fast-verify: 6 of 6 checks passed (0s), wall 0.55s; only the
    # five intended paths, committed as 79dfbc0

Observed during M2, 2026-09-22, from the repository root:

    ./tools/checks/loop-runner
    # observed: five scenario lines — advance, split, silent, unrecorded,
    # refusal — then "ok — 5 scenarios driven through ./tools/loop-runner,
    # 6 stub sessions invoked, every assertion held.", exit 0, wall 1.74–1.90s

    ls -d ${TMPDIR:-/tmp}/tmp.* | wc -l   # 40 before the run and 40 after it

    ./tools/verify
    # observed: seven per-check report blocks, then "fast-verify: 7 of 7
    # checks passed (2s)."; wall 2.23s, 2.13s, 2.16s over three runs

    sed -i '' '471d' tools/loop-runner      # the sabotage; diff under Artifacts
    ./tools/verify
    # observed: exit 1, "loop-runner: 7 violations across 5 scenarios driven
    # through ./tools/loop-runner.", then "fast-verify: 1 of 7 checks failed."
    # — only the unrecorded scenario failed; the other four still passed

    git checkout -- tools/loop-runner ; ./tools/verify
    # observed: no diff, then fast-verify: 7 of 7 checks passed (2s)

    env GIT_DIR=$PWD/.git GIT_INDEX_FILE=$PWD/.git/index ./tools/checks/loop-runner
    # observed: exit 0, all five scenarios pass — the environment the hook
    # supplies is neutralised inside the check. With that neutralisation
    # disabled the same line failed every scenario and wrote fixture commits
    # into this repository; see Surprises, and the recovery below.

    git reset --mixed 5828ec0 ; git status --porcelain
    # observed: HEAD and index restored, the working tree's seven intended
    # paths intact, the three stray fixture commits unreachable

Observed during M3, 2026-09-22, from the repository root. The smoke checkout
is `$HOME/blueprint-trials/loop-smoke-20260921-220833/repo`, written `<smoke>`
below; it and its log directory are still there:

    claude --version ; codex --version ; omp --version
    # observed: 2.1.278 (Claude Code) / codex-cli 0.150.1 / omp/18.1.14 — the
    # same three banners this plan recorded at authoring

    git -C <smoke> log --oneline --name-only
    # observed: one baseline commit carrying AGENTS.md, CLAUDE.md,
    # plans/PLANS.md, plans/active/smoke.md and the six skills files

    ./tools/loop-runner run <smoke> --limit 2 -- claude -p --model opus --dangerously-skip-permissions
    # observed: exit 0, 173s wall; two iterations, "2 of 2 iterations
    # advanced plans/active/smoke.md", commits 7ebc00f and 2a7156f, "1
    # Progress entry remains unticked."; no refusal and no halt en route

    git -C <smoke> log --name-only --format='%h %s'
    # observed: 2a7156f listing two.txt and plans/active/smoke.md, 7ebc00f
    # listing one.txt and plans/active/smoke.md, then the baseline

    cat <smoke>/one.txt <smoke>/two.txt ; ls <smoke>/three.txt
    # observed: one, two; and "No such file or directory" for three.txt,
    # with the third Progress entry still "- [ ] M3 — create three.txt …"

    stat -f '%N %m' <logs>/01-iteration.txt <logs>/02-iteration.txt
    # observed: 1790046727 and 1790046799 against the run's start epoch
    # 1790046626 — iterations of 101s and 72s

    ./tools/verify ; git status --porcelain
    # observed: fast-verify: 7 of 7 checks passed (3s), wall 2.41s; the one
    # modified path is plans/active/watched-loop-runner.md, this file

## Validation and Acceptance

The plan is done when a person who has never seen it can, in this order: run `./tools/verify` and watch seven checks pass, the seventh being the loop-runner self-test exercising the stop rule against fixtures (or six plus a recorded `built`-branch measurement); read `docs/capabilities/index.md` and find `loop-runner` off `specced` with an honest enforcement column; run the M3 invocation shape against their own checkout-with-a-plan and watch milestones advance one commit-visible step at a time until the limit; find no `D5` in `docs/DEBT.md`; and find `GOALS.md`, `docs/MATURITY.md`, and the card agreeing that this is the watched runner and that no rung was claimed. Each milestone's own acceptance clauses above are the per-session checks; every one is a command plus what it must print, and every recorded result must be observed output, never expectation.

## Idempotence and Recovery

Every fixture and log lives outside the worktree in `mktemp -d` scratch or under `$HOME/blueprint-trials/`, so a failed run deletes its directory and starts over; scratch is never reused, matching the blueprint-eval rule. The runner never edits the target — sessions do — so a halted run leaves the target exactly as its last session's commits left it, and re-running after a fix resumes at the same milestone by construction (the next unticked entry). The sabotage step in M2 is restored with `git checkout -- tools/loop-runner` and verified green before anything is committed. If M3's environment refuses all three harnesses that day, the milestone stops with the refusals recorded and Progress split per the convention — refusal evidence is not passing evidence. If any session here nears its context limit, split the milestone in Progress (completed/remaining), log the split, commit, and stop.

## Artifacts and Notes

Authoring preflight, observed 2026-09-21:

    fast-verify: 6 of 6 checks passed (0s).   # wall 0.60s
    2.1.278 (Claude Code)
    codex-cli 0.150.1
    omp/18.1.14

The refusal catalogue grounding the halt design (each recorded as observed evidence in the named completed plan): Claude Code credits refusal — exit 0, no commit (`blueprint-live-trial.md`); Claude Code session limit — exit 1 (`decide-procedure-location.md`); Codex CLI flag conflict exit 2, model-version refusal exit 1, usage-limit exit 1, untrusted-directory exit 1, and the meaningless stdin banner (`blueprint-live-trial.md`). omp recorded no refusals across its runs.

M1's hand-run transcripts, observed 2026-09-22, with the scratch root elided from the paths — the fragments that decide each acceptance clause:

    loop-runner: 2 of 2 iterations advanced plans/active/fixture.md in …/A.
      iteration 1: "(2026-09-22 02:36Z) M1: create one.txt containing one" — commit 132243f, log …/logsA/01-iteration.txt
      iteration 2: "(2026-09-22 02:36Z) M2: create two.txt containing two" — commit ab1dce9, log …/logsA/02-iteration.txt
    1 Progress entry remains unticked. Review the plan, then re-run to continue.

    loop-runner: halted after iteration 2 of 3.
    plans/active/fixture.md milestone "M2: acceptance unobservable — create nothing" did not complete: the session exited 1 and left 1 commit; no Progress entry became complete.
    Read that entry and …/logsB/02-iteration.txt, fix the cause, and re-run; the loop resumes at the same milestone.

    loop-runner: halted after iteration 1 of 2.
    plans/active/fixture.md did not advance: the session exited 0 and produced no commit.
    The last lines of …/logsD/01-iteration.txt say what the harness did instead; fix the cause and re-run.

    loop-runner: refusing to start.
    plans/active/fixture.md has a completed Progress entry with no timestamp at line 8 ("M1: create one.txt containing one").
    A run cannot distinguish work it did from work it inherited unless the record is current. Update the plan, then re-run.

M2's sabotage, observed 2026-09-22. The edit, a single deleted line in the judging block of `tools/loop-runner`, which is what "continue iterating when no Progress entry flipped" costs:

    @@ -468,7 +468,6 @@
      unstamped=$(awk -F'\t' '$1 == "NEWDONE" && $3 == "unstamped" …

      if [ "$newdone" -eq 0 ]; then
    -   faults=$((faults + 1))
        facts="$facts; no Progress entry became complete"
      elif [ "$newdone" -gt 1 ]; then

What `./tools/verify` then reported, abridged to the deciding lines — note that four of the five scenarios still passed, which is the discovery this milestone recorded:

    loop-runner: unrecorded — the run exited 0 where 1 is the only correct answer.
      The run stops at the first iteration whose milestone did not complete, naming the plan, the entry, and the transcript to read. …
    loop-runner: unrecorded — the run never printed "loop-runner: halted after iteration 1 of 2.".
    loop-runner: unrecorded — sessions invoked: expected 1, observed 2.
    loop-runner: unrecorded — commits above the baseline: expected 1, observed 2.
      what that run printed:
        …
        loop-runner: 2 of 2 iterations advanced plans/active/fixture.md in …/unrecorded.
          iteration 1: "" — commit e0d1157, log …/logs-unrecorded/01-iteration.txt
          iteration 2: "" — commit 75ecc7f, log …/logs-unrecorded/02-iteration.txt
    loop-runner: 7 violations across 5 scenarios driven through ./tools/loop-runner.
    fast-verify: 1 of 7 checks failed.

After `git checkout -- tools/loop-runner`, `./tools/verify` printed `fast-verify: 7 of 7 checks passed (2s)` again. The sabotage was never committed.

The scratch directories are not kept. M2 rebuilt the same four fixtures and the same stub inside `tools/checks/loop-runner`, added the fifth, and that is where they stopped being disposable.

M3's watched run, observed 2026-09-22, with the trial root elided. The runner's
own summary, which is the acceptance:

    loop-runner: 2 of 2 iterations advanced plans/active/smoke.md in …/repo.
      iteration 1: "(2026-09-22 03:10Z) M1 — create one.txt holding the word one." — commit 7ebc00f, log …/01-iteration.txt
      iteration 2: "(2026-09-22 03:12Z) M2 — create two.txt holding the word two." — commit 2a7156f, log …/02-iteration.txt
    1 Progress entry remains unticked. Review the plan, then re-run to continue.

The smoke plan's Progress after the run, which is the same judgement read from
the target rather than from the runner:

    - [x] (2026-09-22 03:10Z) M1 — create one.txt holding the word one.
    - [x] (2026-09-22 03:12Z) M2 — create two.txt holding the word two.
    - [ ] M3 — create three.txt holding the word three.

The working rule the smoke checkout's `AGENTS.md` carries, which is what a
project driven by this runner has to say somewhere and what this repository
does not yet say anywhere:

    - One commit per milestone, carrying both that milestone's work and the
      plan edit recording it. A commit that leaves plans/active/smoke.md
      untouched is not part of this project's history.

## Interfaces and Dependencies

Everything below is a contract for the executing sessions; later sessions reuse these names and shapes rather than inventing parallel ones.

The runner. `tools/loop-runner`, POSIX shell, no dependencies beyond git and the utilities the existing checks already use (`awk`, `sed`, `grep`, `mktemp`, `date`).

    ./tools/loop-runner run <repo-dir> [--plan <repo-relative-path>] --limit <n> [--logs <dir>] -- <session command...>

    exit 0  ran to the limit or to plan completion, every iteration clean
    exit 1  halted: an iteration violated the stop rule
    exit 2  refused to start, or cannot decide (bad arguments, dirty or
            unresolvable target, stale record, target inside this worktree)

`<repo-dir>` must be a git worktree outside this repository's worktree. `--plan` defaults to the single `*.md` under the target's `plans/active/`; zero or several is a refusal that says so. `--limit` is a required positive integer counting attempted iterations. `--logs` defaults to a fresh sibling directory `<repo-dir>-loop-logs-<yyyymmdd-hhmmss>`; an existing directory is refused, never reused. Iteration i streams to `<logs>/<0i>-iteration.txt` via tee, with the session's exit status captured through a status file since POSIX sh has no pipefail.

Per-iteration protocol. Preconditions, checked before every iteration: target clean per `git status --porcelain`; plan file present at the resolved path; the structural contract of `docs/capabilities/evidence-check.md` holds for that one file (four sections present and non-empty, ticked entries timestamped, fenced and indented material ignored — reimplemented as one awk pass in the runner's own body); at least one unticked entry remains, else print the plan-complete summary and exit 0. A precondition failing before iteration 1 is a refusal (exit 2); failing after a session ran is that iteration's halt (exit 1). Session: from the target directory, run the session argv with the brief appended as one final argument, stdin `</dev/null`, output teed live. Judgement, from `start=HEAD` before and `end=HEAD` after: at least one new commit; every commit in `git log --name-only <start>..<end>` touches the plan path; comparing checkbox lines of `git show <start>:<plan>` and `git show <end>:<plan>`, exactly one entry flipped unticked-to-ticked, that entry contains a `(YYYY-MM-DD` timestamp, and no entry reverted; session exit status 0. Any miss halts with the matching message; otherwise continue.

The brief, embedded in the runner verbatim (adapted from the proven trial brief; `<plan>` is the resolved repo-relative path):

    You are working in a git repository whose agent harness is already
    installed, and which has a plan in flight at <plan>. Nothing outside
    this repository is available to you, and you must not go looking for it.

    Task: execute the first unfinished milestone of that plan, following the
    repository's own execution procedure, and then stop.

Message shapes (the card's bar: name the plan, the milestone or defect, and the resume path). Halt on an unfinished milestone:

    loop-runner: halted after iteration <i> of <n>.
    <plan> milestone "<entry text>" did not complete: <observed facts — the
    session's exit status, its commit count, and that no Progress entry
    became complete / the entry was split in place>.
    Read that entry and <logs>/<0i>-iteration.txt, fix the cause, and
    re-run; the loop resumes at the same milestone.

Halt on a session that committed nothing:

    loop-runner: halted after iteration <i> of <n>.
    <plan> did not advance: the session exited <s> and produced no commit.
    The last lines of <logs>/<0i>-iteration.txt say what the harness did
    instead; fix the cause and re-run.

Refusal on a stale record, before any session runs:

    loop-runner: refusing to start.
    <plan> <defect — e.g. has a completed Progress entry with no timestamp
    at line N>. A run cannot distinguish work it did from work it inherited
    unless the record is current. Update the plan, then re-run.

Summary on success:

    loop-runner: <k> of <n> iterations advanced <plan> in <repo-dir>.
      iteration 1: "<entry text>" — commit <sha>, log <logs>/01-iteration.txt
      ...
    <m> Progress entries remain unticked. Review the plan, then re-run to
    continue.  (or: the plan has no unticked entries left.)

Fixtures, used by M1 by hand and by `tools/checks/loop-runner` forever. Each is a git repository holding one file `plans/active/fixture.md` with the four living sections non-empty and a three-entry Progress; the check builds them in `mktemp -d` scratch. Fixture A, the passing shape:

    - [ ] M1: create one.txt containing one
    - [ ] M2: create two.txt containing two
    - [ ] M3: create three.txt containing three

Fixture B is A with the second entry reading `M2: acceptance unobservable — create nothing`; the word `unobservable` is the stub's trigger. Fixture C is A with the first entry pre-ticked and carrying no timestamp. The stub session, `stub-session`, one POSIX script: it appends its final argument (the brief) to `brief-received.txt` so the check can assert the brief plumbing; finds the first unticked entry of `plans/active/fixture.md`; if that entry contains `unobservable`, it rewrites the entry as a split (completed: nothing; remaining: the original text), commits `session: split`, and exits 1; otherwise it creates the named file with the named word, ticks the entry with `(date -u +"%Y-%m-%d %H:%MZ")`, commits `session: advance`, and exits 0. In the refusing scenario the stub would `touch` a sentinel file; the check asserts the sentinel absent.

Fixture D, the silent-refusal shape, added by the pre-execution review: A with the second entry reading `M2: silent refusal — the session will do nothing`; the word `silent` is the stub's trigger for it. On a `silent` entry the stub appends the brief to `brief-received.txt` as always — proving the session was genuinely invoked — then creates nothing, commits nothing, and exits 0, which is the exit-zero credits refusal reproduced in miniature. The runner must halt on the absent commit, not the exit status, and its message is the committed-nothing shape from the message contract above. Fixture D's first entry is pre-ticked with a timestamp so the silent entry is the first the runner attempts.

Fixture E, the unrecorded-advance shape, added by the M2 session because the sabotage this plan specifies is invisible without it: A with the first entry reading `M1: unrecorded advance — commit prose without ticking`; the word `unrecorded` is the stub's trigger. On such an entry the stub appends one line of prose to the plan, commits `session: prose`, and exits 0, so the iteration is clean in every respect the runner measures except the one that matters — one commit, that commit touching the plan, exit 0, and no entry complete. It is the only fixture whose halt depends on the record rule alone.

As built, the five fixtures live under names rather than letters — `advance`, `split`, `silent`, `unrecorded`, `refusal`, each its own directory under the scratch root — because the check reports per scenario and a letter in a report line says nothing to whoever reads the failure. The stub is one script with the scratch root baked in at generation time, four branches keyed on `silent`, `unrecorded`, `unobservable` and anything else, and one line of its own output per invocation so that each iteration's transcript proves the tee plumbing carried it.

The check. `tools/checks/loop-runner`, same output conventions as its siblings: one report line per scenario stating what was exercised and what held, a final summary line, exit 0 all held / 1 violation / 2 cannot decide, scratch removed on every exit path. Wired as the last word of the CHECKS list in `tools/verify`. As built it adds three things the contract above did not anticipate: it unsets every inherited `GIT_*` variable and exports its own committer identity before touching a fixture, because the hook's environment otherwise redirects fixture commits into this repository; it prints the failing run's own transcript, indented, after a scenario's violations, since the scratch it came from is gone by then; and it prints a remediation only when it differs from the previous one, so that one broken behavior reads as one reason and several facts.

Close-out drafts, so M4 edits rather than composes. `GOALS.md` non-goal replacement for the current "No background automation in v1…" bullet:

    - No unattended automation in v1. The watched runner `tools/loop-runner`
      drives milestone sessions under an explicit iteration limit, halts on
      anything unexpected, and leaves a human reviewing at plan boundaries;
      running it with nobody watching, scheduling it, and recurring GC agents
      stay out of scope, and building it claims no maturity rung.

M2 consumed part of this draft: the index row below is landed, and the Current-rung status contradiction is corrected. What is left of the maturity edit for M4 is the watched wording that arrives with the `GOALS.md` rewrite.

`docs/MATURITY.md` Current-rung sentence: rewrite the clause "and `loop-runner` gates a rung two steps away" to state the status M2 landed (enforced or built), that the tool runs watched, and that the rung it gates is still not claimed — the Gating capabilities table stays untouched. Card Enforcement point second paragraph: replace "Nothing enforces it today: …" with a sentence naming `tools/checks/loop-runner` under `./tools/verify` and `tools/hooks/pre-commit` (or the on-demand `built` wording if M2's budget branch fired), keeping the pointers to `docs/capabilities/index.md` for status and `docs/MATURITY.md` for the rung, and ending with the v1 boundary now recorded in `GOALS.md`: invoked watched, under a limit, no rung claimed. `docs/capabilities/index.md` row:

    | `loop-runner` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |

Decision record: `docs/decisions/0027-the-loop-runner-is-watched-and-judges-by-commit-shape.md`, graduating the watched-scope and commit-shape-judgement decisions from this log, in the format `docs/decisions/DECISION_FORMAT.md` defines.

What this plan deliberately leaves out: unattended or scheduled operation and any change to the Gating capabilities table or rung claim (the L2 question stays exactly where `docs/MATURITY.md` left it); session resumption or retry inside an iteration (a halt is a human's to read — one session, one judgement); driving plans in this repository's own checkout (refused by design, revisitable by its own decision); harness-specific flags inside the runner (they live in the invocation the human supplies); and any `template/` change (the payload ships no loop-runner card, and `tools/` is live-only by the layer map — a target project that wants a runner writes its own card, which is the blueprint's contract for every enforcer).

## Revision Notes

- 2026-09-22 (pre-execution review): added fixture D — the zero-commit,
  exit-0 session — to the stub contract, M1's acceptance (now seven items),
  M2's scope and scenario counts, and Concrete Steps. Reason: the halt this
  design exists for, the exit-zero refusal that heads the plan's own
  catalogue, was the one behavior no scenario produced; its drafted message
  had no observation scheduled. Milestone boundaries and all other
  contracts unchanged.

- 2026-09-22 (M1 execution): recorded M1's observed evidence in Progress,
  Surprises & Discoveries, Concrete Steps and Artifacts and Notes; added five
  decisions taken while building (entry identity by normalised text, lazy log
  directory creation, the two refusal vocabularies, the early `docs/specs/index.md`
  covers edit, and the stub writing outside the target); wrote the first
  Outcomes & Retrospective entry. Reason: the House Rules require each session
  to leave the record current and the next session to be able to start from
  this file alone. No contract under Interfaces and Dependencies changed, and
  the milestone boundaries are as authored.

- 2026-09-22 (M2 execution): recorded M2's observed evidence in Progress,
  Surprises & Discoveries, Concrete Steps, Artifacts and Notes and Outcomes &
  Retrospective; added four decisions taken while building (the fifth fixture,
  the neutralised git environment, the deliberately unpinned guards, and the
  early maturity and count corrections); added fixture E and the as-built
  notes to the fixture and check contracts; changed M2's scope and acceptance
  from four scenarios to five. Reason: the specified sabotage turned out to be
  undetectable by the four authored scenarios, which is a contract change the
  next session must read rather than rediscover, and the House Rules require
  the record to be current at the stopping point. Milestone boundaries are as
  authored; M3 and M4 are untouched except for the one sentence of M4's
  maturity draft that M2 already satisfied.

- 2026-09-22 (M3 execution): recorded M3's observed evidence in Progress,
  Surprises & Discoveries, Concrete Steps, Artifacts and Notes and Outcomes &
  Retrospective; replaced the Expected block under Concrete Steps with the
  observed one; added three decisions taken while rehearsing (the smoke
  checkout stating the commit shape, timings derived from log mtimes, and the
  trial artifacts kept). Reason: the House Rules require the record to be
  current at the stopping point, and the commit-shape finding is a gap in what
  this repository ships that M4 or a later plan has to answer rather than
  rediscover. No contract under Interfaces and Dependencies changed, the
  runner and its check were not edited, and M4 is as authored.
