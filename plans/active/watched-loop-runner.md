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
- [ ] M1 — `tools/loop-runner` exists; passing, halting, and refusing behavior each observed by hand against scratch fixtures; `AGENTS.md`, `ARCHITECTURE.md`, and `docs/specs/mechanical-checks.md` name the new command.
- [ ] M2 — `tools/checks/loop-runner` self-test encodes the card's three scenarios, runs under `./tools/verify` inside the budget, has been seen to fail against a sabotaged runner, and the index row moves off `specced`.
- [ ] M3 — a real harness drives two milestones of a real plan in a smoke checkout outside this tree, watched, with the transcript and commit shapes recorded here.
- [ ] M4 — close-out: `D5` deleted, `GOALS.md` non-goal reworded, `docs/MATURITY.md` narrative updated, card's Enforcement point rewritten, decision graduated, plan moved to `plans/completed/`.

## Surprises & Discoveries

- Observation: Claude Code moved from `2.1.274` (recorded at trial authoring five days ago) through `2.1.278` (after the operator re-authenticated mid-trial) and still prints `2.1.278 (Claude Code)` today; Codex CLI and omp are unchanged.
  Evidence: `claude --version` → `2.1.278 (Claude Code)`, `codex --version` → `codex-cli 0.150.1`, `omp --version` → `omp/18.1.14`, observed 2026-09-21 while authoring. Harness version drift between authoring and M3 is expected and is why M3 re-records the banner on its day.
- Observation: the verification budget has room for a self-test that builds scratch git repositories.
  Evidence: `./tools/verify` observed 2026-09-21, `fast-verify: 6 of 6 checks passed (0s)`, wall time 0.60 seconds against the five-second budget in `docs/capabilities/fast-verify.md`.

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

## Outcomes & Retrospective

Nothing yet. Authored 2026-09-21; no milestone has been executed.

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
4. Refusing case: against fixture C, the runner exits 2 naming the plan and the defect (a completed entry with no timestamp), and the stub's sentinel file proves no session was invoked. Then make fixture A dirty with an untracked `stray` file, re-run the passing invocation, observe the dirty-tree refusal, and delete the stray file.
5. A target inside this worktree is refused: `./tools/loop-runner run . --limit 1 -- true` exits 2 with the inside-worktree message.
6. `./tools/verify` prints `6 of 6 checks passed` and `git status --porcelain` shows only intended files.

### M2 — The self-test, the wiring, and the status

Scope: encode M1's three hand-run scenarios as `tools/checks/loop-runner`, wire it into `./tools/verify`, measure the budget, demonstrate the check failing against a sabotaged runner — the promotion bar in `docs/capabilities/CARD_FORMAT.md` — and move the index row. At the end of this milestone the stop rule is checked on every commit to this repository, or the plan records the measured reason it is not.

The work: the check builds fixtures A, B, and C and the stub in `mktemp -d` scratch (never inside the worktree), runs `./tools/loop-runner` against each, and asserts the observable facts from M1's acceptance: exit codes, the required message fragments, commit counts and touched paths, flip and timestamp shapes, the untouched third entry, and the uninvoked-stub sentinel in the refusing case. One report line per scenario, a summary line, exit 0/1/2, scratch removed on every path. Append `loop-runner` to the CHECKS list in `tools/verify`. Update the check counts in `docs/specs/mechanical-checks.md` (six become seven, plus a paragraph on what this check decides) and the covers column of `docs/specs/index.md`. Move the `loop-runner` row in `docs/capabilities/index.md` to `enforced` at `./tools/verify`, `tools/hooks/pre-commit`.

The sabotage demonstration, in this order, committing none of it: edit `tools/loop-runner` to continue iterating when no Progress entry flipped (the exact edit and diff go in this plan); run `./tools/verify`; observe the loop-runner check fail naming the violated behavior; restore the runner; observe verify green. A self-test never seen red proves nothing, which is the same bar every other card here paid.

Budget branch: record the observed wall time of `./tools/verify` with the check wired in. Inside five seconds, the row reads `enforced` and this branch closes. Over five seconds, remove `loop-runner` from CHECKS, record the measurement, set the row to `built` with `Enforced at` reading `./tools/checks/loop-runner` as the on-demand command, and M4 words the card's Enforcement point accordingly — `docs/capabilities/fast-verify.md` makes the budget part of its invariant, and busting one card to promote another is not available.

Acceptance:

1. `./tools/checks/loop-runner` alone on a clean tree prints three scenario lines and an ok summary, exits 0, and leaves nothing behind in `$TMPDIR`.
2. `./tools/verify` prints `7 of 7 checks passed (Ns)` with the observed N recorded here and judged against the budget (or the `built` branch is recorded with its measurement).
3. The sabotaged-runner run: verify exits 1 with the check's message visible; the restore run: verify green. Both transcripts in this plan.
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

Expected (labelled so; nothing below has been run, and M1 runs each line before recording it):

    # M1 fixture setup, in a scratch directory outside the worktree
    S=$(mktemp -d)
    mkdir -p "$S/A/plans/active" && cd "$S/A" && git init -q
    # write plans/active/fixture.md as fixture A (shape below), commit
    # write $S/stub-session (contract below), chmod +x
    cd <repository root>

    ./tools/loop-runner run "$S/A" --limit 2 --logs "$S/logs" -- "$S/stub-session"
    # expected: exit 0; two iterations; summary naming both entries,
    # commits, and $S/logs; third entry untouched

    ./tools/loop-runner run "$S/B" --limit 3 --logs "$S/logsB" -- "$S/stub-session"
    # expected: exit 1 after iteration 2; halt message naming
    # plans/active/fixture.md, the M2 entry text, what was not observed,
    # and the resume path

    ./tools/loop-runner run "$S/C" --limit 1 --logs "$S/logsC" -- "$S/stub-session"
    # expected: exit 2; refusal naming the unstamped completed entry;
    # $S/sentinel absent

    # M2, from the repository root
    ./tools/checks/loop-runner        # expected: three scenario lines, ok summary, exit 0
    ./tools/verify                    # expected: fast-verify: 7 of 7 checks passed (Ns), N recorded
    # sabotage: edit tools/loop-runner per M2, run ./tools/verify, expect exit 1
    # with the check's message; git checkout -- tools/loop-runner; verify green

    # M3, from the repository root, smoke repo built as the milestone describes
    ./tools/loop-runner run $HOME/blueprint-trials/loop-smoke-<stamp>/repo --limit 2 -- claude -p --model opus --dangerously-skip-permissions
    # expected: exit 0, two milestones advanced by real sessions; on an
    # environment refusal, the omp then codex lines from Context, each
    # refusal transcript recorded

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

The check. `tools/checks/loop-runner`, same output conventions as its siblings: one report line per scenario stating what was exercised and what held, a final summary line, exit 0 all held / 1 violation / 2 cannot decide, scratch removed on every exit path. Wired as the last word of the CHECKS list in `tools/verify`.

Close-out drafts, so M4 edits rather than composes. `GOALS.md` non-goal replacement for the current "No background automation in v1…" bullet:

    - No unattended automation in v1. The watched runner `tools/loop-runner`
      drives milestone sessions under an explicit iteration limit, halts on
      anything unexpected, and leaves a human reviewing at plan boundaries;
      running it with nobody watching, scheduling it, and recurring GC agents
      stay out of scope, and building it claims no maturity rung.

`docs/MATURITY.md` Current-rung sentence: rewrite the clause "and `loop-runner` gates a rung two steps away" to state the status M2 landed (enforced or built), that the tool runs watched, and that the rung it gates is still not claimed — the Gating capabilities table stays untouched. Card Enforcement point second paragraph: replace "Nothing enforces it today: …" with a sentence naming `tools/checks/loop-runner` under `./tools/verify` and `tools/hooks/pre-commit` (or the on-demand `built` wording if M2's budget branch fired), keeping the pointers to `docs/capabilities/index.md` for status and `docs/MATURITY.md` for the rung, and ending with the v1 boundary now recorded in `GOALS.md`: invoked watched, under a limit, no rung claimed. `docs/capabilities/index.md` row:

    | `loop-runner` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |

Decision record: `docs/decisions/0027-the-loop-runner-is-watched-and-judges-by-commit-shape.md`, graduating the watched-scope and commit-shape-judgement decisions from this log, in the format `docs/decisions/DECISION_FORMAT.md` defines.

What this plan deliberately leaves out: unattended or scheduled operation and any change to the Gating capabilities table or rung claim (the L2 question stays exactly where `docs/MATURITY.md` left it); session resumption or retry inside an iteration (a halt is a human's to read — one session, one judgement); driving plans in this repository's own checkout (refused by design, revisitable by its own decision); harness-specific flags inside the runner (they live in the invocation the human supplies); and any `template/` change (the payload ships no loop-runner card, and `tools/` is live-only by the layer map — a target project that wants a runner writes its own card, which is the blueprint's contract for every enforcer).
