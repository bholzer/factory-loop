# Decide where an installed procedure set lives, and re-run the live trial against the decision

This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

This document must be maintained in accordance with `plans/PLANS.md`.

## Purpose / Big Picture

A project bootstrapped from this repository's payload receives a set of
procedures — markdown files an agent reads and follows — and puts them at
`skills/<name>/SKILL.md`. Every agent environment that loads procedures
automatically loads them from a root it picks itself, and none of the three
environments this project supports picks that one. The bootstrap procedure's
step 9 therefore tells a session to "make the installed set reachable there",
naming no location, and leaves the rest to judgement. Three live trials
exercised that sentence and resolved it three ways: one environment got a
link, one got a link in a different place on a second run of the same
environment, and one got nothing at all — correctly, because the only
locations it reads are the machine owner's directories outside the
repository. Nothing in a bootstrapped repository records which happened or
why, and `docs/DEBT.md` carries the open question as `D4`.

After this work, a person reading `skills/harness-init/SKILL.md` finds a rule
that covers all three cases instead of one case and an ellipsis, including
the case where the honest action is to install nothing; a person reading a
bootstrap report finds a sentence naming what was installed, where, or why
nothing was; and `docs/DEBT.md` carries neither `D4` nor `D12`. The change is
to an arriving procedure, so `docs/capabilities/blueprint-eval.md` gates it:
the evidence that it works is three live trials, one per supported
environment, each driving a real project from an empty directory to an
executed milestone. Those same three trials pay the bookkeeping half of
`D13`, which is owed regardless.

Two things this plan deliberately does not do. It does not promote
`blueprint-eval` above `specced`: promotion needs a failing case that an
environment can actually fail, nobody has designed one, and
`docs/decisions/0024-blueprint-eval-can-never-read-enforced.md` caps the card
at `built` in any event. And it does not move the procedures anywhere. Both
exclusions are argued in the Decision Log.

## Progress

- [x] (2026-09-21 22:17Z) Plan authored: five milestones, the `D4` pick made
  and argued, the text contract for both procedure edits fixed, the trial
  briefs' extraction command verified, and the `./tools/verify` baseline
  observed. No procedure edited and no trial run.
- [x] (2026-09-21 22:56Z) M1 — Land the pick in the two procedures. Step 9's
  second clause replaced and step 8's sentence added, both in commit
  `5da4b4c`, with all six acceptance items observed and no carve-out.
  `docs/DEBT.md` is deliberately unchanged: `D4` and `D12` are retired by M5,
  after the trials say the pick survived contact. No trial has judged the new
  text yet.
- [ ] M2 — Claude Code: the whole trial against the edited procedures.
- [ ] M3 — omp: the whole trial against the edited procedures.
- [ ] M4 — Codex CLI: the whole trial against the edited procedures.
- [ ] M5 — Close out: retire `D4` and `D12`, halve `D13`, graduate the
  decision, reflect the behavior.

Use timestamps to measure rates of progress.

## Surprises & Discoveries

Everything below was observed in the repository root, by running the command
named in the entry. The first five entries were observed while authoring this
plan on 2026-09-21; an entry added by a later pass names that pass.

- Observation: the repository is green and fast before any of this work, so a
  later failure is this plan's doing rather than inherited.
  Evidence: `./tools/verify` exited 0, ending `fast-verify: 6 of 6 checks
  passed (0s).`, with `doc-integrity: ok — 390 of 393 references resolved in
  51 artifacts`, `prose-duplication: ok — 45 artifacts compared, 34670
  eight-word windows examined, 20 allowlist entries applied, 0 stale.` and
  `boundary-lint: ok — 491 references examined in 69 files, 3 rules applied
  from ARCHITECTURE.md, no banned mention in 17 payload files or 6
  procedures.` Wall time 0.54 seconds against a published budget of 5.

- Observation: all three environments are still installed at the versions the
  previous trial recorded, so the invocations this plan carries are not
  written against software that has moved.
  Evidence: `claude --version` printed `2.1.274 (Claude Code)`, `codex
  --version` printed `codex-cli 0.150.1`, `omp --version` printed
  `omp/18.1.14`.

- Observation: the trial briefs can be extracted from the completed trial
  plan mechanically, so this plan does not have to reproduce eighty-nine
  lines of prompt text to stay self-contained.
  Evidence: the three `awk` lines under Concrete Steps produced 62, 21 and 6
  lines respectively — the counts the completed record states — with `grep -c
  'harness-blueprint'` printing `0` for each.

- Observation: neither procedure this plan edits names any of the three
  environments today, so the grep that guards the portability rule has a
  clean baseline and can only catch a regression this work introduces.
  Evidence: `grep -niE '\b(claude|codex|omp)\b|\.[a-z]+/skills'
  skills/harness-init/SKILL.md skills/plan-author/SKILL.md` printed nothing
  and exited 1.

- Observation: the trial driver refuses a bare invocation rather than
  defaulting to something, so a mistyped subcommand in a later session is
  visible immediately.
  Evidence: `./tools/blueprint-eval` printed its two usage lines and exited
  2. The trial root it would use on this machine resolves to
  `/Users/<you>/blueprint-trials`, from `${BLUEPRINT_TRIAL_ROOT:-$HOME/blueprint-trials}`.

- Observation (reconciliation pass, 2026-09-21, after the doc-garden pass
  landed): the baseline above has moved, and the whole move is that pass
  rather than anything this plan did.
  Evidence: `./tools/verify` still exits 0, now ending `fast-verify: 6 of 6
  checks passed (0s).` in 0.52 seconds, with `doc-integrity: ok — 396 of 396
  references resolved in 51 artifacts, 2 format documents skipped, 0
  allowlist entries applied, 0 stale.`, 34958 eight-word windows examined by
  `prose-duplication` over the same 45 artifacts and the same 20 allowlist
  entries, and 495 references examined by `boundary-lint` over the same 69
  files. Run from a worktree at the commit this plan was authored on, the
  same command reproduced the older numbers exactly — 390 of 393 with three
  allowlist entries applied, 34670 windows, 491 references — which is what
  identifies the difference as the garden pass's work, the three deleted
  allowlist entries included. M1's run is compared against these numbers,
  not against the entry above.

- Observation (same pass): the unfilled-copy numbers M1's fifth acceptance
  item compares against moved in one part and held in the other two.
  Evidence: `./tools/blueprint-eval new garden-reconcile` followed by `check
  <trial>/repo` printed `layout: ok — 6 procedures`, `fill: 117 marker lines
  in 8 of 23 markdown files.` and `references: ok — 149 of 151 references
  resolved inside the copy, 1 allowlist entry applied, 0 stale.`, exiting 1
  on the fill part as an unfilled copy must. The same two commands from the
  authoring commit's worktree printed 144 of 146, with the layout and fill
  lines identical. Three of the files a trial copies were edited by the
  garden pass — `template/AGENTS.md`, `skills/doc-garden/SKILL.md` and
  `skills/plan-execute/SKILL.md` — and their diff adds five lines carrying a
  backticked reference and removes none, which is exactly the five the copy
  gained.

- Observation (same pass): the other three authoring-time observations
  survive the garden pass unchanged, so only the two above needed a second
  reading.
  Evidence: the `awk` extraction printed 62, 21 and 6 lines with `grep -c
  'harness-blueprint'` printing 0 for each; the portability grep over the two
  procedures this plan edits printed nothing and exited 1; `./tools/blueprint-eval`
  with no subcommand printed its two usage lines and exited 2. Neither
  procedure this plan edits was among the files the garden pass touched.

- Observation (M1, 2026-09-21): the two edits moved nothing a check measures
  except the quantity of prose, which is the evidence that neither wording
  introduced a path reference or a restatement somebody else owns.
  Evidence: `./tools/verify` exited 0 in 0.59 seconds ending `fast-verify: 6
  of 6 checks passed (0s).`; `doc-integrity` read 396 of 396 references in 51
  artifacts and `boundary-lint` 495 references in 69 files — both identical
  to the pre-edit reading taken at the start of this session — while
  `prose-duplication` examined 35071 eight-word windows against 34958 before
  the edit, the same 45 artifacts, the same 20 allowlist entries, 0 stale,
  and `git status --porcelain tools/allow/` printed nothing.

- Observation (M1): an unfilled copy built from the edited tree reads exactly
  the three lines the reconciliation pass measured, so the +113 windows above
  are the only difference the edits made to anything checkable.
  Evidence: `./tools/blueprint-eval new decision-smoke` printed `copied 25
  files — 19 payload files and 6 procedures` at
  `/Users/<you>/blueprint-trials/decision-smoke-20260921-175628`, and
  `check <trial>/repo` printed `layout: ok — 6 procedures, each with SKILL.md
  and matching frontmatter name.`, `fill: 117 marker lines in 8 of 23
  markdown files.` and `references: ok — 149 of 151 references resolved
  inside the copy, 1 allowlist entry applied, 0 stale.`, exiting 1 on the
  fill part as an unfilled copy must.

- Observation (M1): the portability grep still decides clean after the edit,
  which it can only do because both clauses describe what an environment
  reads rather than naming one.
  Evidence: `grep -niE '\b(claude|codex|omp)\b|\.[a-z]+/skills'
  skills/harness-init/SKILL.md skills/plan-author/SKILL.md` printed nothing
  and exited 1, as it did before the edit. `git diff` for the commit shows
  13 changed lines in the bootstrap procedure, all inside step 9's second
  clause, and 5 added lines in the authoring procedure, all inside step 8.

## Decision Log

- Decision: the procedures keep their canonical location at
  `skills/<name>/SKILL.md`, and per-environment reachability stays something
  the bootstrap installs in the environment it is running in. The other two
  candidates `docs/DEBT.md` `D4` names are rejected.
  Rationale: the first rejected candidate — configure each environment to
  scan the repository's procedure directory — needs a procedure body that
  names configuration files and keys belonging to particular environments,
  which `ARCHITECTURE.md`'s `no-unprovided-skill-target` rule forbids, such a
  path being one the payload does not ship, and
  `docs/decisions/0008-portability-lowest-common-denominator.md` decided
  against; and one of the three reads only roots outside any repository, so
  in-repository configuration cannot reach it at all. The second — move the
  canonical location into a dotted directory the environments share — fails
  on the evidence that no such shared directory exists: the links observed
  were written under two different dotted directories, while the third
  environment's roots were its own system and plugin caches. Moving
  would also break the map line the bootstrap writes, the cross-references
  inside all six procedures, and the allowed-target list in
  `ARCHITECTURE.md`'s layer map, in exchange for a root that one environment
  of three reads. What remains is the candidate that ships, and it is the
  only one with a controlled measurement behind it: in the environment probed
  on both sides, a bootstrapped copy carrying the link named all six
  procedures at startup where the same copy without it named none.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: the pick is landed as three named outcomes in step 9 rather than
  as one instruction, and the third outcome is to install nothing.
  Rationale: the trial produced exactly three behaviors and the text
  anticipates one. A session in an environment whose roots lie outside the
  repository has to derive, unaided, that writing into them would configure
  the machine rather than the repository — one session did derive it, and the
  next one may not. Naming the outcome is what turns a silent judgement into
  a followed rule, and it costs one sentence.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: step 9 also requires the outcome to be named in the report the
  procedure already ends with, and the requirement is written once, in step
  9, rather than added to step 10 or 11 as well.
  Rationale: the same environment resolved the clause two different ways on
  two runs and a bootstrapped repository holds no record of either, so the
  next reader cannot tell a deliberate choice from an accident.
  `docs/PRINCIPLES.md`'s one-owner rule is why the sentence is not repeated
  in the report step: a second statement of the same obligation is the pair
  that goes stale.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: `D12` is paid in the same milestone as the `D4` pick, by adding
  its one sentence to `skills/plan-author/SKILL.md` step 8.
  Rationale: `D12`'s trigger is "the next pass that edits the authoring
  procedure for another reason, or the next run of the live trial", and this
  plan is the second of those. Its cost is the re-driven sessions that
  `docs/decisions/0025-a-payload-fix-mid-trial-invalidates-the-harness-results-before-it.md`
  requires of any procedure edit, and this plan is already paying that cost
  for the bootstrap procedure in all three environments. Folding it in adds
  two driven sessions in one environment — the one whose authoring and
  execution records currently stand — where deferring it buys three whole
  trials later for one sentence. The alternative considered and rejected was
  keeping this plan's subject pure; purity here costs roughly ninety minutes
  of driven session time the next time anyone opens the question.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: the bookkeeping half of `D13` is folded into this plan's
  validation, and the design half is left open.
  Rationale: `D13`'s first half asks for one end-to-end trial per environment
  against the payload as it stands, of which one is in hand. This plan's gate
  asks for a trial per environment against the payload as this plan leaves
  it, which is a superset: the edit invalidates the one standing bootstrap
  record too, so the three runs are owed either way and doing them once
  discharges both. The second half — a failing case that an environment can
  actually fail — is not folded in, because every environment observed so far
  reaches a procedure by reading the tree, so designing the case means first
  observing an environment that does not, and this plan creates no such
  observation. Attempting it here would produce an invented breakage and a
  milestone with no acceptance.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: the procedure edits land first, in M1, and every trial runs after
  them.
  Rationale: `0025` retires any harness result recorded against text that
  then changes. A trial run before M1 would be invalidated by M1 and would
  have to be run again, so the order is not a preference but the difference
  between three driven trials and six.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: `docs/capabilities/index.md` still reads `specced` for
  `blueprint-eval` when this plan closes, however well the three runs go, and
  M5 states that as a decision rather than leaving it as an omission.
  Rationale: `docs/capabilities/CARD_FORMAT.md` gates `built` on a
  demonstrated failing case, and the card's harness-side failing case came
  back clean the one time it was run — the environment bootstrapped a copy
  whose entry-point file had been renamed, by reading the file. That case is
  the open half of `D13` which this plan does not take. `0024` independently
  caps the card below `enforced`. A status edit here would claim a gate
  nobody has seen close.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: `docs/decisions/0023-skill-discovery-holds-when-a-session-finds-the-procedure-itself.md`
  is left untouched even though its Rationale cites a debt row this plan
  deletes, and the new record names it instead.
  Rationale: `docs/decisions/DECISION_FORMAT.md` is append-only and permits
  repairing a citation only when a path has stopped resolving.
  `docs/DEBT.md` still resolves; what changes is what that file contains, and
  rewriting the argument to match would destroy the evidence of what was
  known when the criterion was written. The new record at `0026` closes the
  loop from the other end.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: this plan applies `D12`'s remedy to its own command lines before
  the sentence exists in the procedure.
  Rationale: the row's own generalisation is that a statement about composed
  behavior checked one part at a time is not checked, and a plan that adds
  the rule while carrying an unparsed command line of its own would be the
  third instance of the same defect. Every command line this plan carries as
  an authoring-time claim was run to its first refusal or to completion, and
  the point reached is recorded beside it under Concrete Steps.
  Date/Author: 2026-09-21, plan authoring session.

- Decision: both candidate wordings under Interfaces and Dependencies were
  adopted as written rather than improved, and the replacement was wrapped so
  that the clause's first two lines stay byte-identical.
  Rationale: the candidates already satisfy every content bullet the contract
  states, so an improvement pass would have changed text nobody could then
  compare against the contract that authorised it. The wrapping is what makes
  M1's first acceptance item decidable by a different person: `git diff`
  shows the changed lines beginning at "there without moving it" and ending
  at "why nothing was", so the claim that the first clause and every other
  step are untouched is read off the diff rather than argued.
  Date/Author: 2026-09-21, M1 execution session.

- Decision: `docs/specs/bootstrap-flow.md` was left describing step 9's old
  behavior in this milestone, even though the procedure it describes changed
  in this commit.
  Rationale: M5 owns that file and needs the three trials' outcomes to write
  it, and correcting it now would state which outcome each environment
  produces before any environment has run against the new text — the exact
  substitution of expectation for observation this plan's evidence rules
  forbid. The cost is one milestone's window in which the spec is behind the
  procedure, carried openly here rather than silently.
  Date/Author: 2026-09-21, M1 execution session.

## Outcomes & Retrospective

Nothing has been executed. This section is written at M5, comparing what the
three trials observed against the purpose stated at the top of this file, and
naming what the plan left open.

## Context and Orientation

Read this section if you have never worked in this repository. It assumes
nothing.

This repository is a document system: no code compiles here and no test suite
runs. It has two halves. `template/` is the payload — the artifact set a
target project receives by a plain recursive copy and then fills in.
`skills/` holds six procedures — `harness-init`, `plan-author`,
`plan-execute`, `doc-garden`, `retro`, `capability-build` — each a single
markdown file at `skills/<name>/SKILL.md` with `name` and `description`
frontmatter, which an agent reads and follows. The repository root is the
payload filled in for this project, which makes this repository the first
client of its own bootstrap flow. `AGENTS.md` is the map; `ARCHITECTURE.md`
states which half may reference which.

Terms used below, defined once. An *environment* is an agent command-line
program a session runs inside; this project supports three, named in
`GOALS.md` as a success condition, and they are Claude Code, omp, and Codex
CLI. A *trial* is a throwaway git repository built outside this tree from
both halves, in which an agent is driven through bootstrap, plan authoring
and one executed milestone with no access to this repository. A *probe* is a
read-only session in such a copy whose only job is to report which procedures
the environment handed it at startup and from which directory. A *fill slot*
and a *guidance block* are the two kinds of authoring scaffolding the payload
ships and a bootstrap is supposed to remove; the syntax is defined in
`template/AGENTS.md`.

The subject of this plan is one sentence in one procedure.
`skills/harness-init/SKILL.md` step 9 has two clauses. The first says that if
the environment does not read `AGENTS.md` on its own, add one root file under
the name it does read, holding nothing but a reference — this clause works
and is not touched. The second currently reads, in full: "Second, if that
environment loads procedures only from a location of its own choosing, make
the installed set reachable there — by its configuration where one exists,
otherwise by a link — and change nothing about the files themselves. Their
location is the environment's convention; the map's reference by path is what
keeps them findable when no convention applies."

What three trials observed about that sentence, recorded in
`docs/DEBT.md` `D4` and in the completed plan at
`plans/completed/blueprint-live-trial.md`: no environment surfaced any of the
six arriving procedures at startup in any run before a bootstrap installed
anything;
every bootstrap session nonetheless found the right procedure within its
first two commands by listing the tree and reading a file; one environment's
session wrote a link under a dotted directory and a later run of the same
environment wrote a link under a different dotted directory; and a third
environment's session installed nothing, because the roots it reads are the
machine owner's own directories rather than anywhere inside a project. One
controlled measurement exists: in the environment probed on both sides, the
bootstrapped copy carrying the link named all six procedures at startup,
where the same copy before the link named none. So the shortfall costs
discovery latency, not access.

Three artifacts constrain what may be written into a procedure.
`ARCHITECTURE.md`'s layer map allows a file under `skills/` to name only
paths the payload provides plus sibling procedures, and forbids naming any
environment-specific tool; that fourth clause has no mechanical check and is
tracked as `D10`, so it is a reading job during review of any procedure edit.
`docs/decisions/0008-portability-lowest-common-denominator.md` fixes the
file shape and the rule that procedure bodies are procedures over files,
shell and git only. `docs/PRINCIPLES.md` holds the one-owner rule that keeps
the same obligation from being stated twice.

Three decisions constrain how this work is validated.
`docs/capabilities/blueprint-eval.md` is the card whose invariant is that a
plain recursive copy of both halves is sufficient to carry one feature from
plan authoring to an executed milestone in every supported environment, with
no access to this repository; its enforcement point is a trial run, gating
any change under `template/` or `skills/`; its acceptance is five
observations per environment plus a demonstrated failing case.
`docs/decisions/0023-skill-discovery-holds-when-a-session-finds-the-procedure-itself.md`
settles how the first of those five is judged: it holds when a session given
only the trial repository and the environment's default configuration names
and then follows the right procedure, with nothing created or edited anywhere
to make the procedures visible, and with the operator's prompt naming no
procedure and no filename or path among the arrived artifacts. A permission
or model flag is recorded with the invocation rather than counted against it.
`docs/decisions/0025-a-payload-fix-mid-trial-invalidates-the-harness-results-before-it.md`
is why M1 comes first: a fix under either half retires every recorded result
that judged the text it changed, scoped to the sessions that read that text —
a bootstrap-procedure edit retires bootstrap records and leaves authoring and
execution records standing.

Two debt rows are paid here and one is halved. `D4` is the open question this
plan decides. `D12` is one missing sentence in `skills/plan-author/SKILL.md`
step 8: an expected command line in a plan is never parsed before the session
that has to run it, which the previous trial discovered when a driver
invocation composed from individually correct flags was refused by argument
parsing on first contact. `D13` has two halves — the bookkeeping half, one
end-to-end trial per environment against the payload as it stands, and the
design half, a failing case an environment can actually fail. This plan pays
the first and leaves the second.

The tools. `./tools/verify` is the cheap verification command: it runs six
checks under `tools/checks/` in a fixed order inside a five-second budget and
exits nonzero if any reports a violation or cannot decide.
`./tools/blueprint-eval` is the trial driver and is not a check:
`new <label>` copies both halves into a fresh git repository under
`${BLUEPRINT_TRIAL_ROOT:-$HOME/blueprint-trials}/<label>-<timestamp>/` with
`repo/` and `logs/` inside it, commits the copy as "Receive the payload", and
prints where; `check <repo-dir> [layout] [fill] [references]` decides three
scriptable parts of the card — that the procedures arrived in the expected
layout, that no authoring scaffolding survived the bootstrap, and that every
path reference inside the copy resolves inside the copy — exiting 0 when they
hold, 1 on a violation, 2 when it cannot decide. Neither command is run by
the other.

The target project the trials build is `tally`, a dependency-free Python
command-line tool that records labels in a file inside its own checkout and
prints counts. It is specified by the three briefs the previous trial wrote
and this plan reuses verbatim; the same target in all three environments is
what makes the three records comparable.

## Plan of Work

M1 edits two files under `skills/` and nothing else. In
`skills/harness-init/SKILL.md`, the second clause of step 9 is replaced by a
clause naming three outcomes and a reporting obligation; the first clause,
the step's opening sentence, and every other step are untouched. In
`skills/plan-author/SKILL.md`, step 8 gains one sentence requiring an
expected command line to be run to its first refusal before it is written
down. Both edits are constrained by the layer map: no environment name, no
dotted directory, no tool name. The exact content contract for both is under
Interfaces and Dependencies. Nothing in `docs/` changes in M1 — the debt rows
stay until the trials say the pick survives contact, which is the whole point
of running them.

M2, M3 and M4 are one environment each and are identical in shape. Build a
fresh trial with the driver. Extract the three briefs from the completed
trial plan into the trial's `logs/` directory. Run a read-only probe in the
fresh copy. Drive three sessions — bootstrap, author, execute — each a
separate process with standard input closed and no resumption. Run the probe
again in the bootstrapped copy. Run the driver's three parts after the
bootstrap and again after the executed milestone. Record the five card
observations, the two this plan adds about step 9's outcome and step 8's
sentence, and the timings.

M5 retires what the work paid for: `D4` and `D12` leave `docs/DEBT.md`
entirely, `D13` loses its first half, the pick graduates to
`docs/decisions/0026-…`, `docs/specs/bootstrap-flow.md` stops describing the
question as open and describes the decided behavior instead, and `GOALS.md`
and `docs/MATURITY.md` state what withholds promotion after three more runs.
The card's status is re-affirmed as `specced` in writing.

A finding in any milestone is recorded, not smoothed over. If a trial exposes
a defect in either half, fix it where it belongs and append a re-run
milestone for every environment whose record that fix invalidates, per
`0025`. If the fix is bigger than the session's remaining room, split the
milestone in place in `Progress`, log the split, and stop.

## Milestones

### M1 — Land the pick in the two procedures

What exists at the end that did not exist before: a bootstrap procedure whose
step 9 covers the case where the honest action is to install nothing, and
which requires whatever happened to be named in the report; and an authoring
procedure that tells an author to run an expected command line before writing
it down. No trial has judged either yet.

Acceptance, all observable by a different person:

1. `skills/harness-init/SKILL.md` step 9's second clause names three
   outcomes — configuration, a link, or nothing — states that the third
   applies when every location the environment reads lies outside the
   repository, gives the reason that a bootstrap configures the repository it
   runs in and not the machine it runs on, and requires the outcome to be
   named in the procedure's closing report. Step 9's first clause and every
   other step are byte-identical to their previous state: confirm with
   `git diff` showing changed lines only inside that clause.
2. `skills/plan-author/SKILL.md` step 8 carries one sentence requiring an
   expected invocation to be run to the first refusal the environment can
   produce without doing the work — argument parsing, authentication, a
   version banner — with the point it reached recorded beside it.
3. `grep -niE '\b(claude|codex|omp)\b|\.[a-z]+/skills'
   skills/harness-init/SKILL.md skills/plan-author/SKILL.md` prints nothing,
   as it did before the edit. This is the portability clause the layer map
   leaves to review, made decidable for the two words that would actually
   appear.
4. `./tools/verify` exits 0 ending `fast-verify: 6 of 6 checks passed`, with
   `prose-duplication` reporting no violation and no new entry added to
   `tools/allow/prose-duplication.txt`, and `boundary-lint` still reporting
   no banned mention in the six procedures.
5. A trial built from this state with `./tools/blueprint-eval new
   decision-smoke` passes the layout and references parts and fails only the
   fill part, which is what an unfilled copy must do. Record the three
   summary lines and the file counts; measured against the tree as it stands
   after the garden pass, under Surprises, an unfilled copy reads `layout:
   ok — 6 procedures`, `fill: 117 marker lines in 8 of 23 markdown files.`
   and `references: ok — 149 of 151 references resolved inside the copy, 1
   allowlist entry applied, 0 stale.`, and a difference is a finding to
   explain rather than a failure by itself. The two edits this milestone
   makes carry no path reference in either candidate wording, so a moved
   references count means the wording introduced one.
6. `docs/DEBT.md` is unchanged: `D4` and `D12` are still there, and this
   milestone's Progress entry says why — the rows are retired by M5, after
   the trials.

The work. Replace the clause; add the sentence; run the checks. Keep both
edits in one commit, because they are one gated change and the three trials
judge them together.

### M2 — Claude Code: the whole trial against the edited procedures

What exists at the end: a trial repository in which Claude Code took an empty
project from a received payload to a working `tally` command, under the
edited procedures, with its step 9 outcome recorded.

Acceptance:

1. Three driven sessions, each a separate process with standard input closed
   and no resumption, each exiting 0, with the invocation, the version string
   from `claude --version`, the date, the wall time and the commit each one
   made recorded here. The previous run of this environment took 18 minutes
   for the three together.
2. `./tools/blueprint-eval check <trial>/repo` after the bootstrap: the
   layout part passes naming six procedures, the fill part comes back empty
   over the copy's markdown files, and the references part's output is
   recorded with the composition of whatever dangles. Dangling references to
   the target's not-yet-written code are expected at this stage and are not a
   failure; the same command after the executed milestone is where the
   references observation is judged, and its result is recorded either way.
3. The copy carries a feature to a running command: in `<trial>/repo`,
   `python3 -m tally add build && python3 -m tally add build && python3 -m
   tally add ship && python3 -m tally report` prints one line per label with
   counts and exits 0, and the project's own verification command as
   published in the copy's `AGENTS.md` runs and passes. Record both outputs.
4. The discovery observation under `0023`'s criterion, with its evidence: the
   probe in the fresh copy and the probe in the bootstrapped copy, each
   naming how many procedures the environment handed the session and from
   which roots; one or two transcript lines showing how the bootstrap session
   named the procedure it followed; and `ls -a <trial>/repo` showing that the
   copy arrived with no environment configuration directory.
5. The step 9 outcome, which is this plan's own question: which of the three
   outcomes the session took, what it created if anything, and the sentence
   from its report naming it. If the session installed something, the
   post-bootstrap probe says whether the six then surfaced; if it installed
   nothing, the report's reason is quoted.
6. The step 8 sentence's effect at authoring: either a command line in the
   authored plan carrying the refusal point beside it, quoted, or the
   authoring session's own statement that every expected command ran clean.
   An authored plan showing neither is a finding about the sentence's wording
   and is recorded as one.
7. Observations 4 and 5 of the card: the diff of the copy's plan file between
   the authoring commit and the execution commit adds at least one line of
   output that the authoring version did not contain — quote it and name the
   commit — and the execution session stopped after one milestone, with the
   later milestones still unticked.

### M3 — omp: the whole trial against the edited procedures

The same, in omp. Acceptance is M2's seven items with the invocation and
version string for this environment; the previous run took 29 minutes of
driven time. Two additions specific to it. This is the environment that
resolved step 9 two different ways on two runs, so item 5 carries the
comparison: what this run wrote, and whether the edited clause made the
choice reproducible rather than incidental. And this environment has
documented flags that control procedure discovery, so the probes record
whether the roots it read are inside the copy or outside it.

### M4 — Codex CLI: the whole trial against the edited procedures

The same, in Codex CLI. Acceptance is M2's seven items with this
environment's invocation and version string; the previous run took 30 minutes
of driven time. The addition is item 5's other branch: this is the
environment whose roots are entirely outside any repository, so the edited
clause's third outcome — install nothing, and say so — is tested here and
nowhere else. Record whether the session took it deliberately, with the
report sentence, rather than arriving at it by omission. Known refusals in
this environment that have nothing to do with the payload are listed under
Concrete Steps; each is recorded with the invocation and none counts against
an observation.

### M5 — Close out: retire the rows, graduate the decision, reflect the behavior

What exists at the end: a debt register with neither `D4` nor `D12`, a `D13`
carrying only its design half, a decision record a later agent can find
without reading this plan, and a behavior spec describing what a bootstrapped
project actually gets.

Acceptance:

1. `grep -n 'D4\|D12' docs/DEBT.md` prints nothing: both rows and both
   Details sections are gone, and the register's remaining rows keep their
   identifiers unchanged — identifiers are never reused or renumbered.
2. `D13`'s row and Details carry only the undesigned failing case. The
   bookkeeping half is gone because it was paid, and the row says what the
   three runs established. Its trigger names what would make the design half
   possible: an environment observed reaching a procedure through a loader
   rather than by reading the tree.
3. `docs/decisions/0026-<slug>.md` exists in the four-field format
   `docs/decisions/DECISION_FORMAT.md` defines, stating the location pick and
   the three-outcome rule as a rule binding future work, naming the two
   rejected candidates and what they would have cost, and naming `0023` as
   the record whose open item it closes. `0023` itself is unedited.
4. `docs/specs/bootstrap-flow.md` no longer says that automatic surfacing
   does not work and that `D4` carries why; it states what a bootstrapped
   project gets, including which of the three outcomes each environment
   produced, and its "what has not been observed" paragraph is corrected to
   what remains true after three more runs.
5. `GOALS.md`'s Known unknowns and `docs/MATURITY.md`'s Current rung name
   what still withholds promotion after three more runs — the failing case
   nobody has designed — and neither is left offering the previous trial as
   the current evidence. `docs/capabilities/index.md` still reads `specced`
   for `blueprint-eval`, and this milestone's entry states that as a decision.
6. `./tools/verify` exits 0 ending `fast-verify: 6 of 6 checks passed`, this
   file's `Outcomes & Retrospective` is written against the purpose at the
   top, and the file has moved to `plans/completed/`.

## Concrete Steps

All commands run from the repository root unless a working directory is
given. `<trial>` stands for the directory `./tools/blueprint-eval new`
printed.

The baseline, observed 2026-09-21 while authoring:

    ./tools/verify
    # observed: exit 0, 0.54s wall, ending
    # fast-verify: 6 of 6 checks passed (0s).

    grep -niE '\b(claude|codex|omp)\b|\.[a-z]+/skills' skills/harness-init/SKILL.md skills/plan-author/SKILL.md
    # observed: no output, exit 1. This is the M1 item 3 check; it must still
    # print nothing after the edit.

    claude --version ; codex --version ; omp --version
    # observed: 2.1.274 (Claude Code) / codex-cli 0.150.1 / omp/18.1.14

M1, observed 2026-09-21 in this order, after the two edits and before the
commit for the first two, after it for the last two:

    ./tools/verify
    # observed: exit 0, 0.59s wall, ending
    # fast-verify: 6 of 6 checks passed (0s).

    grep -niE '\b(claude|codex|omp)\b|\.[a-z]+/skills' skills/harness-init/SKILL.md skills/plan-author/SKILL.md
    # observed: no output, exit 1 — unchanged by the edit.

    ./tools/blueprint-eval new decision-smoke
    # observed: trial at .../decision-smoke-20260921-175628, 25 files copied.

    ./tools/blueprint-eval check <trial>/repo
    # observed: exit 1 on the fill part alone, with the layout, fill and
    # references summary lines matching the reconciliation pass exactly. The
    # three lines are quoted under Surprises & Discoveries.

Building a trial and extracting the briefs. The `awk` lines below were run
while authoring, against `plans/completed/blueprint-live-trial.md`, and
produced the line counts the completed record states:

    ./tools/blueprint-eval new claude-code
    # expected, on the pattern of every previous run:
    # blueprint-eval: trial at /Users/<you>/blueprint-trials/claude-code-<timestamp>
    # blueprint-eval: copied 25 files — 19 payload files and 6 procedures — committed as "Receive the payload".

    for b in bootstrap author execute; do
      awk -v want="logs/brief-$b.md" 'index($0,"Written verbatim to") && index($0,want) { grab=1; next } grab && /^    / { while (pend-- > 0) print ""; pend=0; sub(/^    /,""); print; seen=1; next } grab && /^[[:space:]]*$/ { if (seen) pend++; next } grab && seen { exit }' plans/completed/blueprint-live-trial.md > <trial>/logs/brief-$b.md
    done
    wc -l <trial>/logs/brief-*.md ; grep -c 'harness-blueprint' <trial>/logs/brief-*.md
    # observed 2026-09-21 writing to a scratch directory: 62, 21 and 6 lines,
    # and 0 for each of the three greps. The counts are the check: a brief of
    # another length means the extraction drifted and the trial must not run.

Driving the sessions. Each line is one process; none resumes another.

    cd <trial>/repo && claude -p --model opus --dangerously-skip-permissions "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    # and the same shape for brief-author.md and brief-execute.md, to
    # 02-author.txt and 03-execute.txt. Expected: three exits of 0, roughly
    # 18 minutes for the three. The model flag is deliberate and recorded
    # with the invocation: this machine's configured default refused a run
    # for billing reasons and exited 0 while doing it, so an exit status
    # alone does not prove a session happened — check for a commit.

    cd <trial>/repo && omp -p --auto-approve --cwd <trial>/repo "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    # and the same shape for the other two briefs. Expected: three exits of
    # 0, roughly 29 minutes for the three.

    codex exec -C <trial>/repo -m gpt-5.6-sol --approve-for-me "$(cat <trial>/logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee <trial>/logs/01-bootstrap.txt
    # and the same shape for the other two briefs. Expected: three exits of
    # 0, roughly 30 minutes for the three. Three refusals in this
    # environment are known and are not payload defects: the sandbox flag may
    # not be passed beside the approval flag, which implies it; the
    # configured default model is refused by the service for this CLI
    # version, which is why a model is named; and the command refuses to
    # start outside a git repository, which a trial never is because the
    # driver commits the copy. It also prints "Reading additional input from
    # stdin..." with stdin closed, which means nothing.

The discovery probe, run once in a fresh copy built for the purpose and once
in the bootstrapped trial copy, in the same environment as the trial it
belongs to:

    <the environment's own invocation, as above> "Answer in plain text and touch no file: list every skill name that this session was given at startup, and for each one name the directory it was loaded from. If none were given, say so explicitly."
    # expected on the previous record: a list of the machine owner's own
    # procedures and none of the copy's six in a fresh copy; in the
    # bootstrapped copy, all six from the copy's own directory where the
    # session installed a link, and none where it installed nothing.

Reading the result, after the bootstrap and again after the executed
milestone:

    ./tools/blueprint-eval check <trial>/repo
    # expected after a bootstrap: layout ok — 6 procedures; fill ok — no
    # authoring scaffolding; references exit 1 listing the target's reserved
    # code paths, which the first milestone has not written yet.
    # expected after the executed milestone: references resolving, or a
    # named residue recorded as a finding.

If an environment refuses to run non-interactively, cannot authenticate, or
stops on a prompt no flag answers, record what it printed and drive that
environment's sessions by hand with the same briefs pasted in, saying in the
record that they were driven by hand. If it cannot be driven at all, its
milestone is split in place, the reason recorded, and the card's status
question is unaffected — it was never going to move in this plan.

## Validation and Acceptance

The whole plan is accepted when a reader can check five things without
running anything: that `skills/harness-init/SKILL.md` step 9 states a rule
for three outcomes and requires the outcome reported; that three trial
records in this file each name which outcome their environment produced, with
a quoted report sentence and a probe pair as evidence; that each of those
three records carries a `tally` command line and its output, proving the copy
carried a feature to a running command; that `docs/DEBT.md` contains neither
`D4` nor `D12` and that `D13` carries only its design half; and that
`docs/decisions/0026-<slug>.md` states the pick with its rejected
alternatives.

The gate `docs/capabilities/blueprint-eval.md` imposes is met when all five
of its observations are recorded for all three environments against the
post-M1 text, with any shortfall named rather than omitted. A shortfall does
not fail the plan — it fails a claim, and the honest record of which claim is
what the card asks for.

`./tools/verify` passes in this repository at the end of every milestone, and
`git status --porcelain` reports nothing outside this plan file during the
trial milestones: a trial mutates its own throwaway repository and never this
one.

## Idempotence and Recovery

Trial repositories are disposable and are never reused: the driver refuses a
destination that already exists, and a trial that went wrong is abandoned
rather than cleaned. Building another costs one command. Nothing is ever
copied back from a trial into this repository; a defect a trial finds is
fixed here, in the half that owns it, and the next trial rebuilds from the
fix.

The M1 edits are two files and are reverted with `git revert` or
`git checkout --` if the trials show the pick is wrong. That outcome is not
failure: it is the measurement the plan exists to take, and it would be
recorded in the Decision Log with what the runs showed, with `D4` rewritten
rather than deleted at M5.

Re-running a driven session is safe but not free — roughly ten minutes of
unattended time — and produces a second record rather than replacing the
first. Both stay, with the later one labelled.

## Artifacts and Notes

Transcripts live at `<trial>/logs/01-bootstrap.txt`, `02-author.txt` and
`03-execute.txt`, beside the briefs and the stored probe output. They are
outside this repository and are not durable: the path is recorded here as
provenance, and the lines that prove an observation are quoted in this file,
short and labelled, because a path into a scratch directory proves nothing to
a reader later.

What gets quoted, per environment: the driver's three summary lines after the
bootstrap and after the executed milestone; the one or two transcript lines
showing how the session named the procedure it followed; the report sentence
naming the step 9 outcome; the `tally` output and the project's own
verification command output; and the line the execution session added to the
copy's plan file that it had observed rather than restated. Everything else
stays in the transcript.

## Interfaces and Dependencies

The text contract for `skills/harness-init/SKILL.md` step 9. The step keeps
its opening sentence and its first clause unchanged. The second clause is
replaced by one that states, in the procedure's own words and naming no
environment, no dotted directory and no tool:

- that the installed set is not moved, whatever is done;
- that where the environment reads its own configuration from inside the
  repository, that configuration is what makes the set reachable;
- that where it reads a location inside the repository that the project does
  not ship, a link is created there and committed with the rest of the
  installation;
- that where every location it reads lies outside the repository, nothing is
  installed, because a bootstrap configures the repository it runs in and
  never the machine it runs on, and the map's reference by path is the route
  that always works;
- that whichever of the three happened is named in the report the procedure
  ends with — what was installed and where, or why nothing was.

Candidate wording, which the executing session may improve but whose content
is the acceptance above:

    Second, if that environment loads procedures only from a location of its
    own choosing, make the installed set reachable from there without moving
    it: by that environment's own configuration where it reads one from
    inside this repository, otherwise by a link created there and committed
    with the rest of the installation. Where every location it reads lies
    outside this repository, install nothing — a bootstrap configures the
    repository it runs in, never the machine it runs on — and leave the map's
    reference by path as the route, which is the one every environment can
    follow. Whichever of the three happened, name it in the report this
    procedure ends with: what was installed and where, or why nothing was.

The text contract for `skills/plan-author/SKILL.md` step 8. One sentence is
added to the existing step, which currently ends "Anything you would have to
ask about is a hole to fill now." The sentence states that an expected
command line is run before it is written down, as far as the first refusal
the environment can produce without doing the work — argument parsing,
authentication, a version banner — and that the point it reached is recorded
beside it. Candidate wording:

    An expected command line is run before it is written down, as far as the
    first refusal the environment can produce without doing the work —
    argument parsing, authentication, a version banner — and the point it
    reached is recorded beside it. A line composed from individually correct
    flags is not a line anyone has run.

Neither edit adds a `Never` entry: both rules are instructions with a place
in the procedure already, and `docs/PRINCIPLES.md`'s one-owner rule makes a
second statement of the same obligation the copy that goes stale.

The trial briefs are `plans/completed/blueprint-live-trial.md`'s three
verbatim blocks, extracted by the `awk` line under Concrete Steps and written
to `<trial>/logs/brief-bootstrap.md`, `brief-author.md` and
`brief-execute.md`. They are reused unchanged, and the check that the
extraction worked is their line counts: 62, 21 and 6. They live in `logs/`
rather than in `repo/` so that they are not artifacts of the project under
test, and none of them names this repository, any procedure, or any path
among the arrived artifacts — which is what `0023`'s criterion requires of
the operator's prompt.

The driver interface, unchanged by this plan: `./tools/blueprint-eval new
<label>` takes exactly one label that is a plain directory name and builds
`${BLUEPRINT_TRIAL_ROOT:-$HOME/blueprint-trials}/<label>-<timestamp>/`
holding `repo/` and `logs/`; `./tools/blueprint-eval check <repo-dir>
[layout] [fill] [references]` runs all three parts when none is named, exits
0 when every named part holds, 1 on a violation and 2 when it cannot decide.

Identifiers this plan spends: `0026` for the decision record at M5. `0027` is
spent only if a trial produces a second decision that binds future work on
its own, and the reason goes in the Decision Log. Debt identifiers are never
reused: `D4` and `D12` are retired, not reassigned.

Revision note, 2026-09-21, doc-garden pass before execution: five statements
in this plan were corrected against the tree, none of them a change of plan.
The Purpose and the third Decision Log entry said nothing in the repository
records which environment resolved step 9 which way; this repository does
record it, in the completed trial plan and in `D4`, and the gap is that a
bootstrapped repository keeps no such record, which is what step 9 now gains.
The first Decision Log entry attributed the ban on naming environment
configuration paths to the layer map's fourth clause, which is about tool
names; the rule that decides a path is `no-unprovided-skill-target`. The same
entry said two links were observed under one dotted directory; the record
holds three link-writing sessions under two different dotted directories, and
the conclusion — no directory all three environments read — is unchanged.
Context said no environment surfaced the procedures before or after
bootstrap; the bootstrapped copy carrying the link did surface all six, which
the same paragraph states four sentences later. M5's fifth acceptance item
asked two files to stop making a claim neither one makes; it now asks them
for what they actually owe. The corrections were made here rather than left
for the executing session because a stateless session reads this file as
fact.

Second revision note, 2026-09-21, reconciliation after the doc-garden pass:
the pass that corrected the five statements above landed two more commits of
its own afterwards, so this file was read against the tree as it now stands.
No part of the plan's argument changed and no citation stopped resolving —
the repository's reference check reads 396 of 396, this file included. What
moved was measurement, in both places this plan carries a number somebody
else produced. The `./tools/verify` baseline under Surprises is left where it
was observed, with a re-observation recorded beside it, because an
authoring-time reading is evidence about that moment and not a claim about
today. M1's fifth acceptance item is an expectation rather than a
record, so its unfilled-copy reference count was replaced with the measured
one and the older figure survives in the entry that explains it. Both
differences were attributed by re-running the two commands from a worktree at
this plan's authoring commit, which reproduced the original numbers exactly.
The rest of the sweep found nothing to fix, and says so here so that the next
reader does not run it again: `docs/DEBT.md`'s `D4`, `D12` and `D13` still
say what the Decision Log says they say, `docs/specs/bootstrap-flow.md` still
describes automatic surfacing as not working and points at `D4` for why,
`GOALS.md`'s Known unknowns and `docs/MATURITY.md`'s Current rung still owe
exactly what M5's fifth item asks of them, `docs/capabilities/index.md` still
reads `specced` for `blueprint-eval`, `ARCHITECTURE.md` still leaves the
tool-name clause to review as `D10`, and every capability card this file
cites is the live one rather than the payload's copy — which is the
misrouting the same garden pass found three times elsewhere in the tree.
