# Publish the role-and-task guide, graduate the check protocol, and teach the bootstrap to record where the guide lives

This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

This document must be maintained in accordance with `plans/PLANS.md`.

## Purpose / Big Picture

Nothing in this repository explains the system to a person. The knowledge
exists — goals, layer map, plan convention, capability cards, maturity
ladder, six procedures, two behavior specs — but it is written for the agent
that operates inside it, one owning file per fact, and a human meeting the
repository cold has no page that says what the pieces are for, in what order
to touch them, or what it looks like when they work. Three further facts are
worse off than scattered: the contract every mechanical check conforms to
lives only in a completed plan's archive, where nothing outside `plans/` may
reach it; the constant prompts the operator actually types to drive sessions
exist only in conversation, which violates this repository's own rule that
nothing material lives outside the repo; and a project bootstrapped from the
payload carries no record of where its artifact set came from, so the person
who inherits such a project cannot find this documentation at all.

After this plan, a reader opening `docs/guide/index.md` is routed by role
and task to four pages: a narrative overview of the whole system, a
walkthrough for bringing it to a project with the output the recorded trials
say to expect, the operator's runbook including the exact prompts that drive
sessions here, and the path from a capability card to a working enforcer.
The check protocol is readable at `docs/specs/check-protocol.md` as a live
owning contract. And the bootstrap procedure's interview carries one new
optional question — where the installed artifact set came from and where
that upstream keeps its guide — whose answer lands as one line in the
client's own `GOALS.md`. That last change edits an arriving procedure, so
the blueprint-eval gate applies: this plan re-drives one bootstrap session
per supported harness against the edited text and records what each one did,
which also pays down `docs/DEBT.md` `D14` — the step 9 reporting clause that
produced the right action and no sentence about it — in the same gated
round, per the batching lesson the previous procedure-edit plan recorded.

How to see it working: `./tools/verify` exits 0 with the guide and the new
spec in the checked set; the map in `AGENTS.md` names `docs/guide/`; and in
each of three fresh trial repositories, the bootstrap session's report
answers both clauses of step 9 in separate sentences, and the two copies
whose owner answers named an upstream carry a provenance line in their own
goals file while the copy whose answers did not stays silent rather than
inventing one.

## Progress

- [x] (2026-09-22 04:14Z) Plan authored: nine milestones; the file cut for
  the new spec argued against `docs/specs/index.md`'s routing guidance; text
  contracts fixed for both procedure edits and for the three trial briefs;
  the operator's constant prompts captured verbatim under Interfaces and
  Dependencies; authoring-time baselines observed and recorded under
  Concrete Steps.
- [x] (2026-09-22 04:35Z) M1 — `docs/specs/check-protocol.md` written (123
  lines) and verified clause by clause against `tools/verify` (43 lines by
  `wc -l`) and the seven executables under `tools/checks/`; one row added
  to `docs/specs/index.md`; one routing sentence added to
  `docs/specs/mechanical-checks.md`. Observed: `./tools/verify` exit 0 in
  2.25s wall ending `fast-verify: 7 of 7 checks passed (2s).`, with the new
  file in every checked set (doc-integrity 435 of 435 in 54 artifacts,
  prose-duplication 46 artifacts, boundary-lint 72 files); `grep -n 'six'`
  on the spec prints only line 120's "sixty", so no sentence describes the
  retired check state; no allowlist entries were added. The first
  post-write verify failed 1 of 7 — see Surprises.
- [x] (2026-09-22 04:45Z) M2 — `docs/guide/overview.md` written (156
  lines): an intro plus five sections mapping one-to-one onto the five
  narrative beats, every section citing the files it leans on. Observed:
  `./tools/verify` exit 0 in 2.32s ending
  `fast-verify: 7 of 7 checks passed (2s).`, with the page in every
  checked set (doc-integrity 466 of 466 references in 55 artifacts,
  prose-duplication 47 artifacts with 20 allowlist entries applied and
  none added, boundary-lint 73 files); the acceptance grep printed two
  lines from the page, both naming `plans/active/` and `plans/completed/`
  as directories. The first post-write verify failed 1 of 7 — see
  Surprises.
- [x] (2026-09-22 05:03Z) M3 — `docs/guide/adopting.md` (116 lines) and
  `docs/guide/operating.md` (138 lines) written. Observed: `./tools/verify`
  exit 0 in 2.48s ending `fast-verify: 7 of 7 checks passed (2s).`, both
  pages in every checked set (doc-integrity 502 of 502 references in 57
  artifacts, prose-duplication 49 artifacts with 20 allowlist entries
  applied and none added, boundary-lint 75 files); the three prompt
  blocks extracted from `docs/guide/operating.md` are byte-identical to
  the blocks under Interfaces and Dependencies (empty diff over 8
  indented lines each side, no backtick anywhere in them); the
  acceptance grep printed, from the two new files, only the unbackticked
  prompt-block line and one backticked directory-as-directory mention.
  One deviation against acceptance 1's coverage: sizing appears as the
  spec's twenty-to-twenty-nine-minutes-per-harness three-session figure,
  not the per-session five-to-eleven range — see Decision Log. The first
  post-write verify failed 1 of 7 — see Surprises.
- [ ] M4 — `docs/guide/implementing.md` and `docs/guide/index.md` exist;
  `AGENTS.md` maps `docs/guide/`.
- [ ] M5 — Both `skills/harness-init/SKILL.md` edits landed in one commit:
  the per-clause step 9 reporting rule and the optional provenance question.
- [ ] M6 — Claude Code bootstrap re-run against the edited procedure,
  provenance answer supplied, all observations recorded.
- [ ] M7 — omp bootstrap re-run, provenance answer withheld, the
  nothing-invented observation recorded.
- [ ] M8 — Codex CLI bootstrap re-run, provenance answer supplied, the
  install-nothing report sentence observed.
- [ ] M9 — Close out: `D14` deleted, `docs/specs/bootstrap-flow.md` current,
  decision 0028 graduated, retrospective written, file moved to
  `plans/completed/`.

M6, M7 and M8 are order-independent: each builds its own throwaway trial
repository and shares no state with the others. If one harness refuses to
run on a given day, execute another's milestone and return. This is declared
here at authoring time because the previous trial plan learned it mid-flight
and said it should have been declared up front.

## Surprises & Discoveries

Everything below was observed on 2026-09-22 while authoring, by running the
command named in each entry.

- Observation: the baseline is clean and the check set has grown since the
  contracts this plan graduates were written — seven checks now, not six.
  Evidence: `./tools/verify` exited 0 in 2.27 seconds wall ending
  `fast-verify: 7 of 7 checks passed (2s).`, with `loop-runner` reporting
  five scenario lines the older aggregator contract never mentioned. The
  graduated spec must describe the present, not the archive.
- Observation: `AGENTS.md` is 75 lines, so the one map line M4 adds fits
  the ~100-line cap with room to spare.
  Evidence: `wc -l AGENTS.md` printed 75.
- Observation: the brief extraction recorded by the previous trial plan
  still reproduces exactly, so the re-runs can reuse it unmodified.
  Evidence: the `awk` loop under Concrete Steps, run against
  `plans/completed/blueprint-live-trial.md` into a scratch directory,
  produced files of 62, 21 and 6 lines, and `grep -c 'harness-blueprint'`
  printed 0 for each.
- Observation: one harness version has drifted since the recorded trials —
  `claude --version` now prints `2.1.278 (Claude Code)` where every recorded
  result says `2.1.274` — while `codex --version` still prints
  `codex-cli 0.150.1` and `omp --version` still prints `omp/18.1.14`. The
  re-runs record the strings they observe; the drift is noted so nobody
  reads a mismatch as a transcription error.
- Observation: `tools/checks/boundary-lint` states in its own header that,
  unlike the reference check, it reads fenced and indented blocks like any
  other line — but what it extracts as a reference is a backticked path, so
  a bare unbackticked path inside a quoted block is invisible to it. This is
  what makes it safe for the runbook to quote, verbatim, an operator prompt
  that names a completed plan file: the quoted line carries no backticks.
  Evidence: the comment block at the top of `tools/checks/boundary-lint`,
  and `docs/decisions/0021-a-path-that-must-not-resolve-is-not-backticked.md`,
  which is the convention the contract below leans on.
- Observation: `./tools/blueprint-eval` with no arguments refuses with a
  two-line usage and exit 2, which is as far as the driver can be exercised
  without building a trial.
  Evidence: the transcript under Concrete Steps.
- Observation: the harness-name guard on the bootstrap procedure is clean at
  baseline, so M5 inherits a meaningful before/after check.
  Evidence: `grep -niE '\b(claude|codex|omp)\b|\.[a-z]+/skills'
  skills/harness-init/SKILL.md` printed nothing, exit 1.
- Observation (M1 session, 2026-09-22 04:34Z): the graduated spec's first
  draft collided with a procedure, not with a card or the archive. The
  first post-write `./tools/verify` failed 1 of 7: prose-duplication named
  `docs/specs/check-protocol.md` and `skills/capability-build/SKILL.md`
  sharing the window "card s remediation text with the real offender"
  (9 words) — the skill's step 4 and the spec's exit-1 clause state the
  same fact. Rewording the spec's sentence to "built by substituting the
  real offender ... into the remediation text its card publishes" cleared
  it.
  Evidence: the failing run's remediation line, quoted above verbatim in
  the window key; the re-run exited 0 in 2.25s ending
  `fast-verify: 7 of 7 checks passed (2s).`
- Observation (M2 session, 2026-09-22 04:40Z): the essay's first draft
  collided on two windows at once, one of them against five files.
  `./tools/verify` failed 1 of 7: prose-duplication reported six pairs —
  "every artifact is markdown operated on with shell" shared with
  `ARCHITECTURE.md` and, as "is markdown operated on with shell and git",
  with four card files restating that same fact, plus "because the
  failure message is the only documentation" shared with
  `skills/capability-build/SKILL.md`. Attribution could not repair the
  first: the excusing citation must name the other file of each pair, so
  citing `ARCHITECTURE.md` would have excused one pair and left four
  standing. Rewording cleared both windows.
  Evidence: the six remediation lines of the failing run, window keys
  quoted above verbatim; the re-run exited 0 in 2.32s ending
  `fast-verify: 7 of 7 checks passed (2s).`
- Observation (M3 session, 2026-09-22 05:03Z): the first post-write
  `./tools/verify` failed 1 of 7 — the third page draft in three
  milestones to collide, this time with the debt register.
  prose-duplication named `docs/DEBT.md` and `docs/guide/adopting.md`
  sharing the window "where every location that harness reads lies
  outside" (10 words shared in total): `D14`'s Details section
  paraphrases step 9's install-nothing branch in the same words the
  page's reachability passage used, and that section of the page cites
  the decision record and the spec but not `docs/DEBT.md`, so the
  attribution class could not carry it. Rewording the page's clause to
  "when the locations that harness reads all sit outside" cleared it.
  Evidence: the failing run's remediation line, window key quoted above
  verbatim; the re-run exited 0 in 2.48s ending
  `fast-verify: 7 of 7 checks passed (2s).`

## Decision Log

- Decision: the guide lives at `docs/guide/` in five files — `index.md`,
  `overview.md`, `adopting.md`, `operating.md`, `implementing.md` — with
  index routing by reader role and task, and it is live-only: no counterpart
  under `template/`, and a client project reaches it through the pointer its
  own bootstrap records rather than by receiving a copy.
  Rationale: owner decision, settled before authoring. The layer map forbids
  the payload to name this repository, so a shipped guide about this
  repository is unshippable by construction; one guide with one owner also
  keeps the payload lean and the documentation current in the only place it
  can be maintained. The live-only choice is exactly the class of question
  `docs/DEBT.md` `D9` says is read by hand, and this entry is the written
  reason the reading will find.
  Date/Author: 2026-09-22, owner decision recorded by the authoring session.
- Decision: the check protocol graduates into a new file,
  `docs/specs/check-protocol.md`, rather than into
  `docs/specs/mechanical-checks.md`.
  Rationale: `docs/specs/index.md` routes an outcome to a new file or to the
  file that already owns that behavior. `mechanical-checks.md` owns what a
  reader can run and what each check decides — subjects and decisions. The
  protocol is a different behavior: the contract any check, present or
  future, conforms to — exit meanings, output shapes, allowlist format, the
  aggregator's streaming and ordering, the budget-setting rule. No live file
  owns it today; it exists only in the completed gate-set plan, where
  nothing outside `plans/` may point. A new owning file keeps both index
  rows narrow, and `mechanical-checks.md` gains one routing sentence.
  Date/Author: 2026-09-22, authoring session, confirming an owner decision.
- Decision: no guide page ever references a specific plan file. Trial
  evidence is reached through `docs/specs/bootstrap-flow.md`, review-practice
  evidence through the `plans/completed/` directory named as a directory,
  and the one operator prompt that names a plan file verbatim appears only
  inside an indented quoted block with no backticks.
  Rationale: `ARCHITECTURE.md`'s `no-plan-file-dependency` rule bans a file
  outside `plans/` from referencing an existing plan file, and
  `tools/checks/boundary-lint` enforces it without a quoted-material
  exemption for backticked paths. The unbackticked form follows
  `docs/decisions/0021-a-path-that-must-not-resolve-is-not-backticked.md`:
  backticks are for citations a checker should chase.
  Date/Author: 2026-09-22, authoring session.
- Decision: the operator's three constant prompts are recorded verbatim in
  `docs/guide/operating.md` as indented blocks, and this plan carries them
  under Interfaces and Dependencies because no file in the repository holds
  them today.
  Rationale: they are owner-supplied facts research cannot recover; losing
  the session that carried them loses them. `GOALS.md` names "No PROMPTS.md"
  as a non-goal, so the tension is settled here rather than discovered
  later: that non-goal rejects a prompt library standing in for skills as
  the reusable entry points. The runbook does the opposite — each prompt
  points a session at the procedure or convention that owns the work
  (`plans/PLANS.md`, `skills/plan-author/SKILL.md`, `tools/loop-runner`),
  and the skills remain the entry points. The owner commissioned exactly
  this content for `operating.md`.
  Date/Author: 2026-09-22, owner decision recorded by the authoring session.
- Decision: the provenance answer lands as one line in the client's
  `GOALS.md` under scope, phrased as a location outside the client
  repository — a URL or a checkout path — never as a repository-relative
  path, and an unanswered question writes nothing.
  Rationale: the bootstrap already routes right-sizing records to
  `GOALS.md`'s scope section, so provenance joins an existing habit rather
  than founding a second one. The phrasing rule keeps the line out of every
  reference checker's jurisdiction: a client that later builds the
  `doc-integrity` card must not inherit a dangling reference its own tree
  can never satisfy — that is the same judgement decision 0021 records. The
  skill text itself may name `GOALS.md`, which the payload ships, but not
  the guide's own path, which it does not — `no-unprovided-skill-target`
  binds the wording, so the guide location reaches the client only inside
  the owners' answer.
  Date/Author: 2026-09-22, authoring session.
- Decision: both procedure edits — the step 9 per-clause reporting rule
  (`D14`'s candidate repair) and the step 2 provenance question — land in
  one commit in M5, before any trial runs.
  Rationale: they are one gated change under
  `docs/capabilities/blueprint-eval.md`, and the completed
  procedure-location plan's retrospective records the lesson in numbers:
  landing the arriving-procedure edit in the first milestone was the
  difference between three driven trials and six. `D14`'s own trigger names
  this exact occasion — the next pass that edits the bootstrap procedure for
  another reason.
  Date/Author: 2026-09-22, owner decision recorded by the authoring session.
- Decision: the re-runs are bootstrap sessions only — one per harness, no
  authoring or execution session.
  Rationale:
  `docs/decisions/0025-a-payload-fix-mid-trial-invalidates-the-harness-results-before-it.md`
  scopes invalidation to the sessions a fix touches: correcting the
  bootstrap procedure invalidates bootstrap records and leaves authoring and
  execution records standing. Both M5 edits live entirely inside the
  bootstrap procedure, and everything they change is observable in the
  bootstrap session's commit and report. M9 writes the same scoping into
  `docs/specs/bootstrap-flow.md` so the surviving records are labelled
  rather than silently mixed with the new ones.
  Date/Author: 2026-09-22, authoring session.
- Decision: the provenance answer is supplied in the Claude Code and Codex
  CLI briefs and withheld in the omp brief.
  Rationale: coverage over ceremony. Two harnesses observe the answer being
  recorded — one that installs a reachability link and one that installs
  nothing, so the pointer is validated beside both step 9 report shapes —
  and one harness observes the decline path, proving the optional question
  writes nothing rather than inventing an upstream. Which harness got which
  role is otherwise arbitrary and is fixed here so the three milestones stay
  order-independent.
  Date/Author: 2026-09-22, authoring session.
- Decision: guide prose restates an owned fact only where the narrative
  cannot point instead, and every section that restates cites the owning
  file in that same section.
  Rationale: owner decision for `overview.md`, generalized to all five pages
  because the enforcement is mechanical either way:
  `docs/capabilities/prose-duplication.md`'s attribution class excuses a
  shared window only when the section it starts in cites the other file, so
  the principle's rule and the check's rule are satisfied by the same
  sentence. Path lists and section-name lists that collide anyway go to
  `tools/allow/prose-duplication.txt` with reasons, per that card's
  judgement class.
  Date/Author: 2026-09-22, owner decision recorded by the authoring session.
- Decision: identifiers. This plan spends decision number 0028 at M9 for the
  live-only-guide-plus-recorded-pointer rule; 0001 through 0027 are spent.
  It deletes debt row `D14` at M9 and creates no debt row unless a milestone
  is forced to defer real work, in which case numbering continues from
  `D16`. Debt identifiers are never reused.
  Date/Author: 2026-09-22, authoring session.
- Decision: the spec names `docs/capabilities/template-live-drift.md` and
  `docs/capabilities/evidence-check.md` as the two checks whose cards
  require per-unit reporting, and describes the five scenario lines from
  `tools/checks/loop-runner` as that script's own report shape rather than
  a card clause.
  Rationale: M1 requires the spec to describe the present, and the present
  streams unit lines from three checks — but
  `docs/capabilities/loop-runner.md`'s acceptance binds the runner's stop
  rule, not the self-test's output shape. Writing the scenario lines in as
  a card requirement would invent a clause no card states; omitting them
  would describe the archive.
  Date/Author: 2026-09-22, M1 session.
- Decision: the duplication collision with the capability-build procedure
  was resolved by rewording the spec, not by allowlisting and not by
  editing the procedure.
  Rationale: `docs/capabilities/prose-duplication.md` reserves the
  allowlist for proper-noun runs, path lists and section-name lists, which
  this window is not; and `skills/capability-build/SKILL.md` is outside
  M1's boundary.
  Date/Author: 2026-09-22, M1 session.
- Decision: M2's duplication collisions were resolved by rewording the
  essay, not by allowlisting and not by adding citations.
  Rationale: neither window is a proper-noun run, a path list or a
  section-name list, so `docs/capabilities/prose-duplication.md` reserves
  no allowlist class for them — the same ground M1's resolution stood on
  — and a citation excuses only the cited pair, which leaves every window
  shared with an uncited third file standing.
  Date/Author: 2026-09-22, M2 session.
- Decision: M3's duplication collision was resolved by rewording the
  page, not by allowlisting and not by adding a citation.
  Rationale: the window is none of the classes
  `docs/capabilities/prose-duplication.md` reserves the allowlist for —
  the same ground M1 and M2 stood on — and citing `docs/DEBT.md` from
  adopting's expectations section would attribute a reachability fact to
  the debt register when the section already names the two files that
  own it, the decision record and the spec.
  Date/Author: 2026-09-22, M3 session.
- Decision: `docs/guide/adopting.md` states the trials' cost as the
  spec's figure — twenty to twenty-nine minutes of driven time per
  harness for the full three-session sequence — rather than the
  five-to-eleven minutes per bootstrap session M3's milestone text asked
  for.
  Rationale: the page-ownership contract under Interfaces and
  Dependencies binds adopting to no claim `docs/specs/bootstrap-flow.md`
  does not back, and the spec records only three-session totals; the
  per-session range exists only in plan records, which no guide page may
  reference. The two instructions collide and the narrower one wins. If
  a per-session figure is worth publishing, M9's spec update — which
  carries the re-runs' bootstrap wall times anyway — is where it gains a
  live owner, and adopting can point at it then.
  Date/Author: 2026-09-22, M3 session.

## Outcomes & Retrospective

Nothing executed yet. This section is written at close-out against the
Purpose above, and earlier if a milestone materially changes what the plan
can deliver.

## Context and Orientation

Read this section if you have never worked in this repository. It assumes
nothing.

This repository is a document system: markdown operated on with shell and
git, no build, no test suite. It has two halves. `template/` is the payload —
the artifact set a target project receives by plain recursive copy and fills
in on arrival. The repository root is that same payload filled in for this
project, which makes this repository the first client of its own bootstrap
flow. `skills/` holds six procedures — `harness-init`, `plan-author`,
`plan-execute`, `doc-garden`, `retro`, `capability-build` — each one markdown
file at `skills/<name>/SKILL.md` that an agent reads and follows.
`ARCHITECTURE.md` states which half may reference which; the rules that
matter to this plan are that nothing under `template/` may name a path
outside the payload, nothing under `skills/` may name a path the payload
does not ship, and nothing outside `plans/` may reference a specific plan
file. `tools/checks/boundary-lint` enforces all three.

`./tools/verify` is the cheap verification command: it runs the seven
executables under `tools/checks/` in a written order and fails if any of
them finds a violation or cannot decide. It runs after every edit this plan
makes, and the versioned hook `tools/hooks/pre-commit` refuses commits it
rejects. Two of its checks shape every word this plan adds.
`tools/checks/doc-integrity` requires every backticked path in the live
artifact half — which includes everything under `docs/`, so every guide page
and the new spec — to resolve, with quoted blocks and allowlisted mentions
excused. `tools/checks/prose-duplication` forbids any eight-word run shared
between two artifacts unless the section it starts in cites the other file,
the window sits in a quoted block, or an allowlist entry in
`tools/allow/prose-duplication.txt` carries it with a reason. The practical
consequence: guide prose is written fresh, points at owners, and cites the
owner in any section that must restate; verbatim material — prompts,
transcripts — is indented, which both checks strip or ignore.

`docs/specs/` holds living descriptions of current behavior, indexed in
`docs/specs/index.md` with one row per file. Two exist:
`docs/specs/bootstrap-flow.md`, which carries what the recorded live trials
established, and `docs/specs/mechanical-checks.md`, which covers the three
commands and what each check decides. `docs/capabilities/` holds one card
per mechanical enforcer with statuses in `docs/capabilities/index.md`;
`docs/capabilities/CARD_FORMAT.md` defines the card sections and the
promotion bar. `docs/MATURITY.md` is the autonomy ladder. `docs/DEBT.md` is
the debt register; row `D14` records that step 9 of the bootstrap procedure
asks for a report of two different outcomes in one sentence, and that in
one of three recorded trials the session that correctly installed nothing
reported nothing about it.

The live trial machinery: `./tools/blueprint-eval new <label>` builds a
fresh git repository outside this tree at
`${BLUEPRINT_TRIAL_ROOT:-$HOME/blueprint-trials}/<label>-<timestamp>/`
holding `repo/` (the copied payload and procedures, committed as "Receive
the payload") and `logs/`; `./tools/blueprint-eval check <repo-dir>` decides
layout, leftover scaffolding, and reference resolution inside the copy. A
trial drives sessions in `repo/` non-interactively with a brief — a prompt
file holding the owner answers a file cannot supply — and the completed
trial plan under `plans/completed/` stores the three brief texts verbatim,
extractable by the `awk` loop under Concrete Steps. The card
`docs/capabilities/blueprint-eval.md` gates any change under `template/` or
`skills/` on a trial run, and decision 0025 scopes what a mid-stream fix
invalidates. Decision 0026 states the reachability rule the previous trial
round validated: the installed procedures never move, and each environment
gets reachability its own way — its committed configuration, a committed
link, or nothing when every location it reads is outside the repository.

The guide this plan writes has one structural constraint worth restating:
`docs/guide/` pages are ordinary live artifacts. They may cite anything the
live half cites — including `template/` — but never a specific plan file,
and their restatements follow the attribution rule above.

## Plan of Work

Nine milestones, one session each. M1 gives the guide its missing pointer
target by graduating the check protocol into a live spec. M2 through M4 build
the guide from the inside out — essay first, task pages second, router and
map line last, so that no committed state ever holds a page citing a page
that does not exist. M5 lands both edits to the bootstrap procedure in one
commit, which arms the gate. M6 through M8 pay the gate: one driven
bootstrap session per supported harness against the edited text, each in a
fresh throwaway repository, each recording the report sentences, the commit
shape, and the presence or deliberate absence of the provenance line. M9
routes the outcomes to their owners — the debt register, the bootstrap-flow
spec, a decision record — and closes the plan.

### M1 — Graduate the check protocol into `docs/specs/check-protocol.md`

What exists at the end: a live file owning the contract a check conforms to
in this repository, indexed, and consistent with the seven checks that run
today rather than the six that ran when the contract was archived.

The content is graduated from the completed gate-set plan's Interfaces and
Dependencies and then verified against the tree, clause by clause, by
reading `tools/verify` and the seven executables under `tools/checks/`. The
spec owns: the invocation contract (POSIX `sh` plus `awk`, `grep`, `sed`,
`sort`, `find`, `cmp`, `test`; no arguments; no network; each script starts
by changing to the repository root so it behaves identically from a
subdirectory and from the hook); the exit semantics — 0 with a final
`<name>: ok` line counting what was examined, 1 with one block per violation
built from the card's remediation text plus per-unit lines for the checks
whose cards require them, 2 with a `cannot run` line naming the reason and
next action, treated by the aggregator as failure; the aggregator contract —
a literal ordered list, never a glob, each child's output streamed
unmodified, the closing summary line shapes for pass and fail; the allowlist
file convention under `tools/allow/` — one entry per line as a key, two
spaces, `#`, a one-line reason, blank and `#` lines ignored, stale entries
counted in the passing summary rather than failed, key shapes owned by each
card; and the budget-setting rule behind the number `AGENTS.md` publishes —
twice the observed clean run rounded up, never above sixty seconds,
re-measured in any commit that changes the check set. Where a clause is also
part of a card's own invariant — the streaming and the exit-2-as-failure
rule sit on `docs/capabilities/fast-verify.md` — the spec's section cites
that card, and the layout inventory stays owned by `ARCHITECTURE.md`'s check
layer entry with a citation rather than a copy.
`docs/specs/mechanical-checks.md` gains one sentence routing readers to the
new file for the conformance contract, and `docs/specs/index.md` gains the
row.

Acceptance:

1. `docs/specs/check-protocol.md` exists and states, in its own prose, every
   clause listed above; each clause about aggregator behavior or ordering
   matches what `tools/verify` actually does, checkable by reading the
   43-line script beside the spec.
2. The spec names both current per-unit reporters and the seven-check order
   as the CHECKS list in `tools/verify` spells it; no sentence describes the
   retired six-check state.
3. `docs/specs/index.md` has one new row whose Covers text does not overlap
   `mechanical-checks.md`'s row, and `mechanical-checks.md` contains one
   routing sentence naming the new file.
4. `./tools/verify` exits 0 ending `fast-verify: 7 of 7 checks passed`, which
   proves the new file's references resolve and its restatements are
   attributed or fresh.

### M2 — `docs/guide/overview.md`

What exists at the end: the narrative essay, readable start to finish by
someone who has never seen the repository, covering: the two halves and the
one allowed direction between them; the six procedures and the occasion each
one exists for; plans as the durable context of a fresh-context loop — why a
session reads a file instead of a conversation; cards to checks to ladder —
how an invariant becomes a specification, then a demonstrated enforcer, then
something a rung can lean on; and the loop — the cheap command after every
edit, the hook, the watched runner, the human at plan boundaries. Every
section points at the file that owns its facts; a restatement appears only
where pointing would break the narrative, and its section cites the owner.

Acceptance:

1. `docs/guide/overview.md` exists and a reader can find each of the five
   narrative beats above as a section or a clearly bounded passage.
2. Every owning file leaned on is cited in the section that leans on it —
   spot-checkable by picking any three factual passages and finding the
   owner named beside them.
3. `grep -rn 'plans/active/\|plans/completed/' docs/guide/` prints no line
   from this file, or only lines naming the directories as directories.
4. `./tools/verify` exits 0 ending `fast-verify: 7 of 7 checks passed`; any
   new `tools/allow/prose-duplication.txt` entries this page needed carry
   reasons and are quoted in this plan's evidence.

### M3 — `docs/guide/adopting.md` and `docs/guide/operating.md`

What exists at the end: the two task pages.

`adopting.md` is the bring-to-a-project walkthrough: what you need (a
checkout of this repository, a target repository, one supported harness);
seeding the target with the payload and the procedures; starting one session
in the target with the owners' answers and letting the bootstrap procedure
run; and what to expect, drawn from the recorded trials by way of
`docs/specs/bootstrap-flow.md`, which owns every observed claim — one
bootstrap commit, a report naming what was created, what was omitted with
reasons, what went to the debt register, and both step 9 outcomes; the
reserved-paths debt row a greenfield bootstrap records instead of a clean
reference walk; five to eleven minutes of driven time per session depending
on harness; then the project's first plan and first milestone as separate
sessions. It names the reachability rule by pointing at decision 0026 and
the brownfield warning by pointing at `D6` in `docs/DEBT.md`, and it states
what the trials observed about discovery: no harness surfaced the arriving
procedures at startup; every session found the bootstrap procedure by
listing the tree, within its first two commands.

`operating.md` documents the actual practice of running this repository, not
a generic role. Its spine: one milestone per session, sessions driven by
three constant prompts recorded verbatim as indented blocks — the
milestone-execution prompt, the plan-authoring prompt, and the watched-loop
line, exactly as fixed under Interfaces and Dependencies; between rounds,
the operator reads the diff and the plan's living sections and lands
findings as reviewer-attributed entries in that plan's Decision Log — before
execution for a freshly authored plan, between milestones for one in flight
— and the evidence that this is practice rather than aspiration is the set
of entries dated with a reviewer author across the plans in
`plans/completed/`, named as a directory. It covers the watched runner from
the operator's chair — what a halt message means, that iterations are judged
by the commits they left rather than by what the session said, pointing at
decision 0027 and the runner's card — the hook install line and the `D8`
caveat that the gate is skippable, when the operator invokes
`skills/doc-garden/SKILL.md` and `skills/retro/SKILL.md`, and the ladder
discipline that no rung moves except by `docs/MATURITY.md`'s promotion rule.

Acceptance:

1. Both files exist with the coverage above, each passage pointing at its
   owner as in M2.
2. The three prompt blocks in `operating.md` match the blocks under
   Interfaces and Dependencies character for character, indented, with no
   backticks inside them.
3. `grep -rn 'plans/completed/' docs/guide/` prints, from these two files,
   only unbackticked lines inside `operating.md`'s quoted prompt block plus
   any line naming the directory as a directory.
4. `./tools/verify` exits 0 ending `fast-verify: 7 of 7 checks passed`; new
   allowlist entries, if any, carry reasons and are quoted in the evidence.

### M4 — `docs/guide/implementing.md`, `docs/guide/index.md`, and the map line

What exists at the end: the enforcer walkthrough, the router, and a map
that names the guide.

`implementing.md` is the path from a card to a working check: read the card
whole and `docs/capabilities/CARD_FORMAT.md` for what each section binds;
in this repository, conform to `docs/specs/check-protocol.md` and wire into
the literal list in `tools/verify` plus the hook; anywhere, follow
`skills/capability-build/SKILL.md` — the procedure is the same in a client
project with the client's own stack; prove the check by its card's failing
case with the remediation text observed, record the evidence in the
authorizing plan and the status in `docs/capabilities/index.md` and nowhere
else.

`index.md` routes: by role — adopter, operator, enforcer-builder, anyone
needing the mental model — and by task, one line each, to the four pages and
outward to `AGENTS.md` as the entry point agents use. It is deliberately the
page a client's recorded pointer lands on, and it says so.

`AGENTS.md` gains one map entry for `docs/guide/` in the docs block of the
Map section, pointing at `docs/guide/index.md`.

Acceptance:

1. Both files exist; every page under `docs/guide/` is reachable from
   `index.md`; `implementing.md` cites the card format, the protocol spec,
   and the build procedure by path.
2. `wc -l AGENTS.md` prints at most 100, and the new line resolves —
   covered by `./tools/verify` exiting 0 ending
   `fast-verify: 7 of 7 checks passed`.
3. `grep -rn 'plans/active/\|plans/completed/' docs/guide/index.md
   docs/guide/implementing.md` prints nothing or only directory mentions.

### M5 — Land both bootstrap-procedure edits in one commit

What exists at the end: `skills/harness-init/SKILL.md` with a step 9 whose
report obligation fires once per clause, and a step 2 interview carrying the
optional provenance question. One commit, both edits: they are one gated
change and the three re-runs judge them together.

The content contracts are under Interfaces and Dependencies; the executing
session may improve the candidate wording but not the content. The step 9
edit replaces only the step's final reporting sentence. The step 2 edit adds
the optional question to the interview paragraph without disturbing the
existing question list or the recorded-unknown rule.

Acceptance:

1. Step 9's text requires a separate report statement per clause — the
   entry-point file added or the reason none was needed, and what was
   installed for reachability and where, or why nothing was — such that a
   session taking the install-nothing outcome still owes a sentence.
2. Step 2's text asks, optionally, where the installed artifact set came
   from and where that upstream keeps its guide; routes an answer to one
   line in `GOALS.md` under scope, phrased as a location outside the
   repository, never repository-relative; and writes nothing when the
   owners decline.
3. `grep -niE '\b(claude|codex|omp)\b|\.[a-z]+/skills'
   skills/harness-init/SKILL.md` prints nothing, exit 1 — unchanged from the
   baseline under Concrete Steps.
4. `./tools/verify` exits 0 ending `fast-verify: 7 of 7 checks passed` —
   in particular `boundary-lint` passes, proving the new wording names no
   path the payload does not ship.
5. `docs/DEBT.md` is untouched in this milestone: `D14` falls only when the
   re-runs have shown the repair firing three times in three.

### M6 — Claude Code: bootstrap re-run with the provenance answer

What exists at the end: a fresh trial repository bootstrapped by Claude Code
under the edited procedure, with every observation recorded here.

Steps: build the trial with `./tools/blueprint-eval new claude-rerun`;
extract `brief-bootstrap.md` with the `awk` loop under Concrete Steps and
confirm 62 lines and zero matches for `harness-blueprint`; append the
six-line provenance block fixed under Interfaces and Dependencies and
confirm 68 lines; drive one session with the observed invocation shape for
this harness; then read the result out of the tree, not out of the report
alone.

Acceptance:

1. One driven session, a separate process with standard input closed and no
   resumption, with invocation, `claude --version` output, date, wall time,
   exit status, and the commit it made recorded here. The previous
   bootstrap-only observations in this harness ran about five to eight
   minutes. Judgement is by the commit, not the exit status.
2. `./tools/blueprint-eval check <trial>/repo`: layout passes naming six
   procedures, fill comes back empty, and the references part's exit and
   dangling composition are recorded — reserved not-yet-written code paths
   are expected and are not a failure.
3. Both step 9 report sentences are quoted from the transcript: the
   entry-point clause's outcome and the reachability clause's outcome, each
   in its own statement.
4. The copy's goals file carries one line naming the upstream location from
   the brief, quoted here with the commit that wrote it, and the line is not
   a backticked repository-relative path.
5. `git show --stat` of the bootstrap commit shows the committed reachability
   link this harness historically writes (a dotted directory pointing back
   at the unmoved procedure directory), and the discovery probe under
   Concrete Steps, run in the bootstrapped copy, reports the six procedures
   offered from the copy's own tree.

### M7 — omp: bootstrap re-run with the provenance answer withheld

The same shape as M6 with this harness's observed invocation, the label
`omp-rerun`, and the unmodified 62-line brief. Differences in acceptance:
item 4 inverts — `grep -n 'blueprint-upstream' <trial>/repo/GOALS.md` prints
nothing, and no other file in the copy names that location either
(`git grep blueprint-upstream` in the copy prints nothing), which is the
observation that an unanswered optional question writes nothing and invents
nothing; item 5 expects this harness's own dotted-directory link and its
probe result. The previous bootstrap-only observations here ran about five
to seven minutes.

### M8 — Codex CLI: bootstrap re-run with the provenance answer

The same shape as M6 with this harness's observed invocation and the label
`codex-rerun`, appending the same six-line provenance block (68 lines).
Differences in acceptance: item 5 inverts — the bootstrap commit contains no
harness configuration and no link, `ls -a <trial>/repo` shows no dotted
harness directory, and no probe is owed, because every location this harness
reads lies outside any repository; and item 3 is the milestone's point —
this is the harness whose previous run took the install-nothing outcome
correctly and reported nothing, so the quoted report must now carry the
reachability clause's own sentence saying nothing was installed and why.
Known refusals in this environment that are not payload defects are listed
under Concrete Steps; each is recorded with its invocation if hit, and none
counts against an observation. The previous bootstrap-only observation here
ran about eleven minutes.

### M9 — Close out

What exists at the end: every outcome routed to its owner, and the plan in
`plans/completed/`.

Acceptance:

1. `grep -n 'D14' docs/DEBT.md` prints nothing: the row and its Details
   section are gone; remaining identifiers unchanged.
2. `docs/specs/bootstrap-flow.md` states the current interview — including
   the optional provenance question and where an answer lands — states that
   the bootstrap report answers step 9's clauses separately, carries the
   re-run dates and which of the three reachability outcomes each harness
   produced, and labels the standing authoring and execution records as
   made against the pre-edit bootstrap text under decision 0025's scoping,
   with the bootstrap-half records superseded by this plan's re-runs.
3. `docs/decisions/0028-<slug>.md` exists in the format
   `docs/decisions/DECISION_FORMAT.md` defines, recording that the adopter
   guide lives in the blueprint and a client records a pointer rather than
   receiving a copy, with the rejected alternative (shipping a guide in the
   payload) and the layer-map reason it loses.
4. `docs/specs/index.md`'s rows still describe their files accurately after
   the edits; any staleness found is fixed in this milestone.
5. `./tools/verify` exits 0 ending `fast-verify: 7 of 7 checks passed`;
   `Outcomes & Retrospective` is written against the Purpose; the file moves
   to `plans/completed/docs-guide.md`.

## Concrete Steps

All commands run from the repository root unless a working directory is
named. `<trial>` stands for the directory `./tools/blueprint-eval new`
printed. Everything in the first block is observed; later blocks are
expected and labelled so.

The baseline, observed 2026-09-22 while authoring:

    ./tools/verify
    # observed: exit 0, 2.27s wall, seven check reports streamed, ending
    # fast-verify: 7 of 7 checks passed (2s).

    wc -l AGENTS.md
    # observed: 75

    grep -niE '\b(claude|codex|omp)\b|\.[a-z]+/skills' skills/harness-init/SKILL.md
    # observed: no output, exit 1

    claude --version ; codex --version ; omp --version
    # observed: 2.1.278 (Claude Code) / codex-cli 0.150.1 / omp/18.1.14
    # note: recorded trials say 2.1.274 for the first; the drift is a
    # harness update, not a transcription error.

    ./tools/blueprint-eval
    # observed: usage on two lines (new <label> / check <repo-dir>
    # [layout] [fill] [references]), exit 2

    for b in bootstrap author execute; do
      awk -v want="logs/brief-$b.md" 'index($0,"Written verbatim to") && index($0,want) { grab=1; next } grab && /^    / { while (pend-- > 0) print ""; pend=0; sub(/^    /,""); print; seen=1; next } grab && /^[[:space:]]*$/ { if (seen) pend++; next } grab && seen { exit }' plans/completed/blueprint-live-trial.md > "$T/brief-$b.md"
    done
    # observed into a scratch $T: 62, 21 and 6 lines; grep -c
    # 'harness-blueprint' printed 0 for each of the three.

M1 through M5 need no commands beyond editing, `./tools/verify` after each
edit, and the two greps their acceptance names.

Building a re-run trial (M6, M7, M8 — expected, on the pattern of every
recorded run):

    ./tools/blueprint-eval new claude-rerun     # or omp-rerun / codex-rerun
    # expected: trial at $HOME/blueprint-trials/<label>-<timestamp>,
    # 25 files copied — 19 payload files and 6 procedures — committed as
    # "Receive the payload".

    # extract brief-bootstrap.md with the awk line above, into
    # <trial>/logs/; expected: 62 lines, grep -c 'harness-blueprint' = 0.

    # append, unindented, the six block lines quoted under Interfaces and
    # Dependencies to <trial>/logs/brief-bootstrap.md, then:
    wc -l <trial>/logs/brief-bootstrap.md
    # expected: 68 (M6, M8) or 62 (M7, untouched)

Driving the session — each line one process, none resumes another; all three
shapes are the ones the recorded trials observed, with only the label and
brief new:

    cd <trial>/repo && claude -p --model opus --dangerously-skip-permissions "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    # expected: exit 0 in roughly 5-8 minutes and one commit over "Receive
    # the payload". This harness has exited 0 on a billing refusal while
    # committing nothing, so the commit is the fact and the exit is not.

    cd <trial>/repo && omp -p --auto-approve --cwd <trial>/repo "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    # expected: exit 0 in roughly 5-7 minutes, one commit.

    codex exec -C <trial>/repo -m gpt-5.6-sol --approve-for-me "$(cat <trial>/logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee <trial>/logs/01-bootstrap.txt
    # expected: exit 0 in roughly 11 minutes, one commit. Known refusals
    # here that are not payload defects: the sandbox flag may not be passed
    # beside the approval flag, which implies it; the configured default
    # model is refused for this CLI version, which is why a model is named;
    # it refuses to start outside a git repository, which a trial never is;
    # it prints "Reading additional input from stdin..." with stdin closed,
    # which means nothing; and account quota can wall every model for days,
    # which splits the milestone in place rather than failing it.

Reading the result (each re-run):

    ./tools/blueprint-eval check <trial>/repo
    # expected: layout ok — 6 procedures; fill ok — no authoring
    # scaffolding; references exit 1 listing only the target project's
    # reserved code paths, recorded as composition.

    git -C <trial>/repo log --oneline ; git -C <trial>/repo show --stat <bootstrap-commit>
    # expected: M6 shows a .claude/skills entry, M7 a .omp/skills entry,
    # M8 neither and no dotted harness directory in ls -a.

    grep -n 'blueprint-upstream' <trial>/repo/GOALS.md
    # expected: one line under scope naming ~/blueprint-upstream and its
    # guide location (M6, M8); nothing at all (M7), corroborated by
    # git -C <trial>/repo grep blueprint-upstream printing nothing.

    # report sentences: quote from ../logs/01-bootstrap.txt the two step 9
    # statements — entry-point clause and reachability clause.

The discovery probe (M6 and M7 only, in the bootstrapped copy, same harness
as the trial; the prompt is one line and touches nothing):

    <that harness's invocation shape above> "Answer in plain text and touch no file: list every skill name that this session was given at startup, and for each one name the directory it was loaded from. If none were given, say so explicitly."
    # expected on the previous record: all six procedures named, loaded
    # from the copy's own tree through the committed link.

If a harness refuses to run non-interactively, record what it printed and
drive that harness's session by hand with the same brief pasted in, saying
so in the record. If it cannot be driven at all that day, split the
milestone in place and execute another of M6-M8 first; they share no state.

## Validation and Acceptance

The plan as a whole is accepted when a reader who has just cloned this
repository can do five things. Open `docs/guide/index.md` from the
`AGENTS.md` map and reach all four pages, each pointing at owners rather
than restating them. Read `docs/specs/check-protocol.md` beside
`tools/verify` and find no clause the script contradicts. Read in
`docs/guide/operating.md` the three prompts the operator actually types,
verbatim, in indented blocks. Read step 2 and step 9 of
`skills/harness-init/SKILL.md` and find the optional provenance question
and the per-clause reporting rule. And read in this file, for each of the
three harnesses, the re-run's invocation, version, date, commit, both
report sentences, and the provenance line present in two copies and absent
in the third — then confirm `./tools/verify` exits 0 ending
`fast-verify: 7 of 7 checks passed` and `docs/DEBT.md` carries no `D14`.

## Idempotence and Recovery

Every live-tree edit here is additive prose or a bounded replacement, safe
to re-run `./tools/verify` against any number of times; a milestone
interrupted mid-edit is recovered by reading the diff against its
acceptance. Trial repositories are disposable and never reused: the driver
refuses an existing destination, so a re-run that went wrong is abandoned
and rebuilt fresh, and nothing a trial does touches this repository's
tracked files. If a re-run surfaces a payload defect, decision 0025
governs: the fix lands as an appended milestone, and every bootstrap record
made against the prior text is re-driven or labelled stale — the same shape
the first trial plan used. Failed session transcripts stay in their trial
`logs/` directories and their evidencing lines are quoted here, because a
scratch path proves nothing later.

## Artifacts and Notes

Trial transcripts live at `<trial>/logs/01-bootstrap.txt` beside the brief;
they are outside this repository and not durable, so every line an
observation rests on is quoted into this file when observed. The previous
round's bootstrap-only wall times, for sizing the re-runs: 4m55s-8m and one
457-second run for the first harness across its recorded bootstraps, 5m26s
and 6m29s for the second, 10m50s for the third.

## Interfaces and Dependencies

These contracts are what a later session cannot rediscover. Any change to
one is a logged decision, not a preference.

The guide file set, fixed: `docs/guide/index.md`, `docs/guide/overview.md`,
`docs/guide/adopting.md`, `docs/guide/operating.md`,
`docs/guide/implementing.md`. No other file joins the directory in this
plan. Page ownership: index owns only routing; overview owns the narrative
and no operational instruction; adopting owns the walkthrough and no claim
`docs/specs/bootstrap-flow.md` does not back; operating owns the practice
and the verbatim prompts; implementing owns the card-to-check path and no
restatement of `docs/capabilities/CARD_FORMAT.md`'s section definitions
beyond attributed mention. No guide page references a specific plan file;
no guide page backticks a path that does not resolve from the repository
root; verbatim material is indented.

The map line M4 adds to `AGENTS.md`, in the docs block of the Map section
(wording adjustable, target not):

    - `docs/guide/` — the role-and-task guide to this system for humans and
      client projects: what it is, adopting it, operating it, building an
      enforcer. Start at `docs/guide/index.md`.

The operator's three constant prompts, owner-supplied, today existing only
in conversation. `docs/guide/operating.md` carries all three verbatim as
indented blocks, character for character:

    Execute the next unfinished milestone of plans/active/<plan>.md,
    following plans/PLANS.md. One milestone, update the living sections,
    commit, stop.

    Read AGENTS.md first. Then follow skills/plan-author/SKILL.md to author
    a plan for <the goal, naming the debt row or commissioning decision and
    the owner facts research cannot supply>. Stop at a committed plan.

    ./tools/loop-runner run <repo-dir> --limit <n> -- <a proven harness
    line from plans/completed/blueprint-live-trial.md>

The text contract for `skills/harness-init/SKILL.md` step 9. Everything
before the final reporting sentence stays. The final sentence — the one
beginning "Whichever of the three happened" — is replaced by wording that
obliges one report statement per clause: for the entry-point clause, the
file added or the reason the environment needed none; for the reachability
clause, what was installed and where, or why nothing was; and that an
outcome that installed nothing is still reported in its own sentence.
Candidate wording, improvable in phrasing but not in content:

    The report this procedure ends with answers the two clauses separately:
    for the first, the entry-point file that was added or the reason the
    environment needed none; for the second, what was installed and where,
    or why nothing was. Installing nothing is an outcome, and it gets its
    own sentence rather than being left to be inferred from the other
    clause's answer.

The text contract for `skills/harness-init/SKILL.md` step 2. After the
interview list and its one-pass instruction, add the optional question:
where the artifact set and procedures being installed came from, and where
that upstream keeps its guide for adopters; an answer becomes one line in
`GOALS.md` under scope, phrased as a location outside this repository — a
URL or a checkout path, never a repository-relative path — and no answer
writes nothing. Candidate wording:

    One question is optional and asked in the same pass: where the artifact
    set and procedures being installed came from, and where that upstream
    keeps its guide for adopters. When the owners name a location, record
    it as one line in `GOALS.md` under scope, phrased as a location outside
    this repository — a URL or a checkout path — so no reference walk here
    is ever asked to resolve it; when they do not, write nothing, because
    provenance nobody supplied is provenance invented.

Neither edit may name a harness, a dotted configuration directory, a tool,
or any path the payload does not ship; `GOALS.md` is namable because the
payload ships it, and the M5 grep plus `boundary-lint` are the checks.

The provenance block appended to the bootstrap brief in M6 and M8 — exactly
these six lines, the leading blank one included, making the brief 68
lines; the upstream named is deliberately fictional and outside every
checked tree, because handing the session this repository's real path would
breach the trial's no-access premise, and the string `blueprint-upstream`
is the grep key the acceptance uses:

    
    One more owner answer, for the optional provenance question: this
    project's artifact set and procedures were installed from the owners'
    blueprint checkout at ~/blueprint-upstream, and that blueprint keeps
    its guide for adopters at docs/guide/ inside that checkout. We would
    like that pointer recorded.

The M7 brief is the extracted 62 lines, untouched.

The driver interface, unchanged by this plan: `./tools/blueprint-eval new
<label>` builds `${BLUEPRINT_TRIAL_ROOT:-$HOME/blueprint-trials}/<label>-<timestamp>/`
with `repo/` and `logs/`; `check <repo-dir> [layout] [fill] [references]`
runs all parts unless one is named, exit 0 when every named part holds, 1
on a violation, 2 when it cannot decide. `./tools/loop-runner` is not used
by this plan.

Identifiers already spent: decision records 0001-0027; debt rows `D2`,
`D6`, `D8`, `D9`, `D10`, `D11`, `D13`, `D14`, `D15` exist today. This plan
spends 0028 (M9), deletes `D14` (M9), and creates debt rows only from `D16`
upward if a milestone must defer real work. Allowlist entries this plan
adds, if any, go to `tools/allow/prose-duplication.txt` in that file's
key-two-spaces-hash-reason shape and are quoted in the milestone's
evidence.

What this plan deliberately does not do. It does not ship a guide in the
payload or edit any file under `template/` — the one payload-adjacent
change is to a procedure under `skills/`. It does not touch
`skills/plan-execute/SKILL.md`, so `D15` stands untouched. It does not
re-run authoring or execution trial sessions, per decision 0025's scoping.
It does not move `blueprint-eval`'s status, which stays `specced` with
`D13` carrying why. It does not claim any rung, and it does not edit
`GOALS.md`: the runbook's relationship to the "No PROMPTS.md" non-goal is
argued in this plan's Decision Log, and the non-goal's text stays true as
written.
