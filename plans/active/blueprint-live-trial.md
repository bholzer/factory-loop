# Run the live trial the blueprint has never had: drive one feature through the payload in Claude Code, omp, and Codex CLI

This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

This document must be maintained in accordance with `plans/PLANS.md`.

## Purpose / Big Picture

Every claim this repository makes about the payload in `template/` and the procedures in `skills/` has been established by reading them from inside the repository that produced them. Six mechanical checks decide structure, correspondence, reference direction and evidence discipline, and all six read the payload from a tree where a dangling reference still resolves against an ancestor and an unstated assumption is still in an author's head. Nobody has ever copied the payload into an empty repository, handed it to an agent with no access to this tree, and watched a feature come out the other end. `docs/DEBT.md` records that as `D7`, `docs/capabilities/blueprint-eval.md` specifies the trial that settles it, and `GOALS.md` lists it first under Known unknowns.

After this plan, an outside reader can see three things that do not exist today. First, a command in this repository, `./tools/blueprint-eval`, that builds a trial repository outside this tree from the payload and the procedures and then decides the three parts of the trial a machine can decide: that the procedures arrived in the layout every supported harness requires, that no authoring scaffolding survived the bootstrap, and that every path reference inside the copy resolves inside the copy. Second, a record in this plan of one real feature — a small command-line tool called `tally`, with a runtime surface a person can exercise from a terminal — carried from bootstrap through plan authoring to an executed milestone, once in each of the three harnesses `GOALS.md` names, with the harness, its version, the date, and the outcome of each of the card's five observations written down as observed. Third, a status for `blueprint-eval` in `docs/capabilities/index.md` that reflects what was observed rather than what was hoped: `built` if the three trials passed and the card's two failing cases were demonstrated, `specced` with the shortfall named if they did not.

Two debt items are touched, one paid and one made actionable. `D7` is paid by running the trial and is deleted when it is. `D4` — the installed procedures sit outside every harness's auto-discovery root — is not decided here; its row names this trial as its trigger, so each harness session in this plan records how it found the procedures it followed, and `D4` is rewritten at close-out to carry those observations instead of the reasoning it carries now. Choosing between the three ways of paying `D4` down is the next plan's work, not this one's.

The observation that makes this plan worth its cost is the one nobody can make from inside this tree: whether a target project that receives a plain recursive copy and nothing else can actually be worked in. The trial target is deliberately a project with a toolchain — a Python package, a test command, a state file, a layering rule — because the payload's toolchain-facing cards, `fast-verify`, `isolated-env` and `boundary-lint`, have never been instantiated anywhere. In this repository they were pruned or filled against a tree of markdown; `GOALS.md` records under Scope that `isolated-env` was not instantiated here at all, because nothing in a repository of documents has a runtime to isolate. The trial is the first time those three cards meet a project that can violate them.

## Progress

- [ ] M1 — Make the trial runnable: build `tools/blueprint-eval` with its `new` and `check` subcommands, demonstrate both of the card's scriptable failing cases, state on the card how the discovery observation is judged, and publish the command in `AGENTS.md` and `ARCHITECTURE.md`.
- [ ] M2 — Claude Code, first half: bootstrap the trial repository from the payload and record observations 1 through 3 with the harness version and date.
- [ ] M3 — Claude Code, second half: author the target's first plan and execute its first milestone in a fresh session; record observations 4 and 5 and the harness's overall outcome.
- [ ] M4 — omp, whole trial in one session: all five observations, the discovery record, and the overall outcome.
- [ ] M5 — Codex CLI, whole trial in one session: all five observations, the discovery record, and the overall outcome.
- [ ] M6 — The discovery failing case: rename the bootstrap procedure's file, observe one harness fail to find it, restore, and set `blueprint-eval`'s status in `docs/capabilities/index.md` to what the four preceding milestones actually observed.
- [ ] M7 — Close out: delete `D7`, rewrite `D4` with the observed discovery facts, update `GOALS.md`, `docs/MATURITY.md` and `docs/specs/`, graduate the durable decisions, and move this file to `plans/completed/`.

Use timestamps to measure rates of progress. Nothing has been executed yet; every entry above is open, and the first session to execute a milestone adds the observation time to its entry in the shape `- [x] (YYYY-MM-DD HH:MMZ)`.

## Surprises & Discoveries

Everything below was observed while authoring this plan, on 2026-09-17, by running the command named in each entry. None of it is a prediction.

- Observation: all three harnesses `GOALS.md` names are installed on this machine and all three can be driven non-interactively, so the trial's harness-driven half does not have to be typed by a person.
  Evidence: `claude --version` printed `2.1.274 (Claude Code)`, `codex --version` printed `codex-cli 0.150.1`, `omp --version` printed `omp/18.1.14`. `claude --help` documents `-p/--print` for non-interactive output, `--dangerously-skip-permissions`, and `--permission-mode`; `codex exec --help` documents `-C/--cd`, `-s/--sandbox` with `workspace-write`, and `--approve-for-me`; `omp --help` documents `-p/--print`, `--cwd`, `--auto-approve`, and — relevant to `D4` — `--no-skills` and `--skills=<glob>`.

- Observation: `codex --help` and `codex exec --help` contain no occurrence of the word "skill", while Claude Code documents skills resolving as `/skill-name` and `~/.claude/skills` exists on this machine, and omp documents skill discovery flags but has no `~/.omp/skills` directory here.
  Evidence: `codex --help | grep -i skill` and `codex exec --help | grep -i skill` both printed nothing; `claude --help | grep -i skill` printed the `/skill-name` and `--disable-slash-commands` lines; `ls -d ~/.claude/skills` succeeded and `ls -d ~/.omp/skills` failed with no such file or directory. This is why the plan records discovery per harness rather than deciding a location: the three harnesses do not have the same mechanism to configure, and one of them appears to have no skills mechanism at all, which makes the `AGENTS.md` map its only route to a procedure.

- Observation: the payload cannot name the procedures, so a bootstrap session cannot find them from the map it arrives with.
  Evidence: `template/AGENTS.md`'s Map section lists ten artifacts and none of them is `skills/`; `ARCHITECTURE.md`'s layer map rule `no-outward-payload-reference` forbids any file under `template/` from mentioning the procedure directory, and `tools/checks/boundary-lint` enforces it. The map line naming `skills/` is added by the bootstrap procedure itself, at its step 8. So in the first session of every trial, discovery cannot come from the map; it comes from the harness surfacing the procedures, or from the session noticing the directory in the tree. Whichever it is, it is a fact about `D4` that only a live session can report.

- Observation: the bootstrap procedure's own Locate step cannot be followed from inside the copy, which is the sequence the card's acceptance prescribes.
  Evidence: `skills/harness-init/SKILL.md` lines 31 to 36 tell the session that the artifacts come from the checkout it was invoked from and that that checkout's `AGENTS.md` map names the directory holding them. In the trial the session is invoked inside the trial repository, whose `AGENTS.md` is the unfilled skeleton and whose map names no such directory, and `docs/capabilities/blueprint-eval.md` requires the trial to run with no access to this repository. The trial therefore performs the copy itself — which the card already states, listing the copy among the scripted parts — and each session is told that the copy and the procedure install arrived already done. Whether the procedure's text should say what a session does when it finds the payload already in place is a finding the trial owes its owners, and each harness milestone records whether the step read as followable.

- Observation: the repository is clean and all six checks pass at the moment this plan was authored, so any failure a later session sees is that session's own.
  Evidence: `git status --porcelain` printed nothing at `3b70703`, and `./tools/verify` ended in `fast-verify: 6 of 6 checks passed (1s).` with `doc-integrity: ok — 367 of 370 references resolved in 47 artifacts, 2 format documents skipped, 3 allowlist entries applied, 0 stale.`, `prose-duplication: ok — 45 artifacts compared, 32554 eight-word windows examined, 20 allowlist entries applied, 0 stale.`, `boundary-lint: ok — 467 references examined in 65 files, 3 rules applied from ARCHITECTURE.md, no banned mention in 17 payload files or 6 procedures.` and `evidence-check: ok — no active plans under plans/active/, 4 sections required of each.` The evidence line moves once this plan lands: the active-plan count becomes one.

- Observation: `tools/allow/doc-integrity.txt` already carries the mention this plan's failing case creates.
  Evidence: the allowlist's three seeded entries include `docs/capabilities/blueprint-eval.md` naming `skills/harness-init/README.md`, the path its failing case tells a builder to create by renaming. M6 creates that file for a few minutes and deletes it; no allowlist edit is needed in either direction, and the entry must not be removed while the rename is in place.

- Observation: the toolchain the trial target needs is present, and choosing a dependency-free target removes the one failure mode that would have had nothing to do with the payload.
  Evidence: `python3 --version` printed `Python 3.10.14` and `pytest --version` printed `pytest 9.0.2`, with `uv 0.12.11`, `node v22.22.2` and `bun 1.3.6` also installed. A target needing `uv sync` or `pip install` needs the network, and Codex's `workspace-write` sandbox denies network access by default, so a sandboxed harness could fail the trial for a reason the payload has nothing to do with. The target therefore uses the standard library and the system `python3` only, and its test command is `python3 -m unittest`.

- Observation: a fresh copy of the payload and the procedures holds 25 files, 146 path references of which 144 resolve inside the copy, and exactly one deliberately unresolvable reference, cited twice; and the eight skeleton-derived artifacts carry 117 marker lines between them.
  Evidence: a throwaway script built a copy with `cp -R template/. repo/` and `cp -R skills repo/skills`, then applied the reference definition `docs/capabilities/doc-integrity.md` owns — backticked spans and link targets containing a slash and none of space, tab, asterisk, angle bracket, brace or colon, outside fenced and indented blocks, over every `*.md` except files ending in `_FORMAT.md` and files under the two plan work directories. It reported 146 references, 144 resolved, and two misses, both `docs/capabilities/doc-integrity.md` naming `docs/NOPE.md` at lines 57 and 59 — the path that card's own failing case tells a builder to create and delete. `grep -rcE` for the two marker shapes reported 17 lines in `AGENTS.md`, 24 in `ARCHITECTURE.md`, 24 in `GOALS.md`, 14 in `docs/PRINCIPLES.md`, 13 in `docs/DEBT.md`, 10 in `docs/MATURITY.md`, 8 in `docs/specs/index.md` and 7 in `docs/capabilities/index.md`, and `ls skills/` in the copy listed the six procedures. These are the numbers M1's acceptance is stated against, and a session that measures different ones has found either a payload change or a defect in the driver.

- Observation: `tools/checks/evidence-check` prints an empty number where a plan has no completed Progress entry yet, which is what this plan produces on the day it lands.
  Evidence: `./tools/verify` with this file in `plans/active/` printed `evidence-check: plans/active/blueprint-live-trial.md — Progress, Surprises & Discoveries, Decision Log and Outcomes & Retrospective all present and non-empty;  of 7 Progress entries complete, every one timestamped.` — two spaces and no zero, because the awk counter is never assigned when nothing is ticked. The check's decision is right and its exit status is right; only the summary line is wrong. It is not this plan's work, and M7 routes it rather than letting it evaporate.

## Decision Log

- Decision: the trial target is `tally`, a dependency-free Python command-line tool that records labels in a file inside its own checkout and prints counts, specified in full under Interfaces and Dependencies and identical in all three harnesses.
  Rationale: the owners asked for a small greenfield project with a real runtime surface rather than another document system, so that the payload's toolchain-facing cards are exercised instead of pruned. `tally` gives each of the three a real subject: `fast-verify` gets a test command that does not exist at bootstrap and must exist by the end of the first executed milestone, `isolated-env` gets a state path that two concurrent checkouts could collide on, and `boundary-lint` gets a layering rule — nothing under `tally/core/` may import `tally.store` or `tally.cli` — that a grep can decide. Standard library only, because a harness sandbox that blocks the network would otherwise fail the trial for a reason unrelated to the payload. The target must be byte-for-byte the same brief in each harness, or the three runs are three different experiments and nothing can be compared.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: the trial worktrees live under `$BLUEPRINT_TRIAL_ROOT`, defaulting to `$HOME/blueprint-trials`, and never inside this repository; `tools/blueprint-eval` refuses a destination inside this worktree.
  Rationale: the card's per-stack hint asks for a directory outside this repository so that a stray reference to an ancestor path fails loudly instead of resolving by accident. A `mktemp -d` directory would satisfy that and was the first choice, but M2 and M3 are two sessions working on the same Claude Code trial repository, and a durable, predictable, stated path is what lets the second session find what the first one built. The worktrees are still scratch: nothing in them is copied back, and a fix they reveal belongs in this repository's payload.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: `tools/blueprint-eval` is not a check under `tools/checks/` and is not added to `tools/verify`'s list.
  Rationale: `docs/decisions/0020-checks-are-live-only-and-state-no-invariant.md` and `ARCHITECTURE.md`'s Check layer entry give `tools/checks/` one meaning — one executable per card, run by the cheap command on every edit. The scriptable parts here decide their questions against a filled copy of the payload, a state that exists only during a trial and never in a commit to this repository, and the questions they would decide against an unfilled copy are already decided on every commit by `boundary-lint`'s `no-outward-payload-reference` rule and by `template-live-drift`. Adding it to the aggregator would put a check in the 5-second budget whose subject does not exist yet. It sits directly under `tools/`, beside the aggregator, and `ARCHITECTURE.md`'s Check layer entry gains it.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: `tools/blueprint-eval` carries its own reference extractor rather than sharing one with `tools/checks/doc-integrity`.
  Rationale: the alternative was to factor the extractor into a file both scripts read, which is the better answer when two owners of one definition is the risk. It loses here for two reasons. `tools/checks/boundary-lint` already implements a third variant of the same idea with a deliberately different reference definition, so a shared extractor would unify two of three and read as an oversight. And the trial's walk runs against a copy that contains no `tools/` directory at all and a filled artifact set that differs from this repository's, so the checked set and the resolution root are both different. The definition's owner stays `docs/capabilities/doc-integrity.md`, and the driver's header comment names that card as the owner and states why the copy exists.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: the discovery observation — "the harness discovered every skill with no per-harness edit to any file" — is judged as follows, and the card is edited in M1 to say so: it holds when a session given only the trial repository and the harness's default configuration names and follows the correct procedure for the task it was given, without the operator's prompt naming a procedure, or a filename or path among the arrived artifacts and procedures, and without any file being created or edited to make the procedures visible. The target project's own intended layout — `tally/core/`, the store path, the module names — is owner interview input and may appear in a brief freely; it is not among the arrived artifacts and names nothing the session must discover. Whether the harness surfaced them automatically, and from which root, is recorded separately as evidence for `D4` and is not a pass or fail of the trial.
  Rationale: `plans/PLANS.md` forbids leaving a decision to an executing session that authoring could settle, and this one would otherwise be settled differently in each of the three harness milestones. The invariant on the card is that the copy is *sufficient to carry one feature*, and a session that reaches a procedure through the map has been carried. Reading it as "surfaced automatically" would fail Codex CLI on a property `D4` explicitly parks — `docs/DEBT.md` `D4` calls the location a per-harness configuration question a live trial answers better than a guess, which is not a question the trial is allowed to answer by failing. The card's failing case still fails under this reading: with `skills/harness-init/SKILL.md` renamed, no route reaches the bootstrap procedure, because the map the payload ships names procedures by the path `skills/<name>/SKILL.md` and nothing else names them at all. A flag that grants the harness permission to write files or reach the network is not a file edit and does not violate the observation; it is recorded with the invocation.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: a payload fix that lands after a harness has passed invalidates that harness's result, and the plan grows a re-run milestone for each invalidated harness instead of keeping the stale record.
  Rationale: `docs/capabilities/blueprint-eval.md` makes the trial the gate on any change under `template/` or `skills/`, so a payload edit made in the middle of this plan is exactly the change the gate exists to catch. Keeping a passing record from before the edit would claim a result about a payload that no longer exists. Re-running is a session, so it is a milestone, appended with the split recorded in `Progress` and the reason in this log.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: the harnesses run in the order Claude Code, omp, Codex CLI, and the first one is cut into two milestones while the other two get one each.
  Rationale: the first trial absorbs every payload defect the trial is capable of finding, and it runs three driven sessions plus a fill check, a reference walk and the evidence write-back, which does not fit one session with room left to record it. The other two run against a payload the first has already flushed. The order runs the harness with the strongest native procedure support first, so that an early failure is likelier to be a payload defect than a harness limitation, and the harness with no skills mechanism at all last, where a failure is unambiguous.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: the card's status may reach `built` at most, never `enforced`, however well the trial goes.
  Rationale: `docs/capabilities/CARD_FORMAT.md` defines `enforced` as running where it cannot be skipped, and three quarters of this card's acceptance is a person invoking a harness and reading what came back. The scriptable quarter could be enforced on its own, but a card states one invariant and carries one status, and this invariant includes the harness-driven observations. Recording `enforced` because part of it runs on every commit would be the manufactured green check `docs/capabilities/CARD_FORMAT.md` warns about. `docs/MATURITY.md`'s L2 row for this card therefore stays ungated after this plan, and the reason is written into the rung section at close-out.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: the next free decision-record identifiers are `0022` onwards, and M7 is the only milestone that spends them.
  Rationale: `docs/decisions/` runs from `0001` to `0021` today, and two sessions each picking "the next number" while a plan is in flight produce two records with the same id. The identifiers are spent at close-out, once it is known which decisions in this log turned out to be durable.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: the references part resolves strictly — the file or directory must exist in the copy — and the deliberate exceptions live in `tools/allow/blueprint-eval.txt` in the same key-and-reason format the other allowlists use, seeded with the one key a fresh copy produces. The parent-directory allowance that `tools/checks/boundary-lint` uses is deliberately not adopted.
  Rationale: `boundary-lint` allows a reference whose parent directory exists, because its question is whether a bootstrapped project would have the directory the path lives in. This card's question is the opposite one and its own failing case depends on the difference: deleting `template/docs/PRINCIPLES.md` leaves a map entry whose parent `docs/` still exists, so under the parent allowance the dangling entry resolves and the failing case cannot fail. Strict resolution makes it fail, at the cost of one allowlist entry for the `docs/NOPE.md` mention measured above. An entry that stops matching is reported as stale in the passing summary, the way `tools/allow/doc-integrity.txt` entries are, so a target project that drops the card decays the entry visibly instead of silently.
  Date/Author: 2026-09-17, plan authoring session.

## Outcomes & Retrospective

Nothing is complete yet, so this section holds no outcome. It is written at M7, and it owes the reader a comparison against the purpose stated above: whether a copy of the payload and the procedures, with no access to this repository, carried one feature to an executed milestone in each of the three harnesses; which of the card's five observations held in which harness; what the trial found that reading the payload could not have found; what was fixed in the payload as a result, and what was routed to `docs/DEBT.md` instead; and what the three harnesses reported about how they found the procedures, which is the input `D4` has been waiting for.

## Context and Orientation

This repository is a document system with two halves. `template/` is the payload: the artifact set a target project receives by a plain recursive copy and fills in on arrival, carrying skeletons with content slots and authoring guidance, two format documents kept verbatim, the plan convention, and a starter set of capability cards. The repository root is that same payload filled in for this project, which makes this repository client number one of its own bootstrap flow. `skills/` holds six procedures an agent reads and follows — `harness-init`, `plan-author`, `plan-execute`, `doc-garden`, `retro`, `capability-build` — each at `skills/<name>/SKILL.md` with `name` and `description` frontmatter. `tools/` holds this repository's own mechanical checks: the aggregator `tools/verify`, six executables under `tools/checks/`, allowlists under `tools/allow/`, and the versioned hook at `tools/hooks/pre-commit`. Nothing under `tools/` is copied into the payload.

Terms this plan uses, defined here because a session executing a milestone may know none of them.

A *harness* is an agent command-line program: Claude Code (`claude`), omp (`omp`), or Codex CLI (`codex`). `GOALS.md` names those three as a success condition, which is what makes them the set this trial must cover.

The *payload* is the contents of `template/`. The *copy* is a trial repository built from it. The *trial repository* is a git repository outside this tree holding the copy plus the procedures; it is scratch, and a defect it reveals is fixed in this repository, never in it.

A *skeleton* is a payload file carrying content slots and authoring guidance, both of which a filled artifact no longer contains. The two marker shapes are defined in `template/AGENTS.md`: a slot is written as two braces, the word FILL, a colon and a description, and every authoring block is an HTML comment whose first word is GUIDANCE. `tools/checks/scaffolding-markers` decides their absence in this repository's own eight filled artifacts; the trial's fill part decides the same thing inside a copy.

A *capability card* is a one-page specification of one mechanical check at `docs/capabilities/<name>.md`, with its status recorded only in `docs/capabilities/index.md` and nowhere else, as `specced`, `built`, or `enforced`. `docs/capabilities/CARD_FORMAT.md` defines what each status requires; promotion to `built` requires someone to have introduced a violation, run the check, and observed it fail with its remediation message visible.

The card this plan runs is `docs/capabilities/blueprint-eval.md`. Read it before starting any milestone; it is one page and it owns the acceptance this plan observes. Its five observations, quoted in the order it states them, are: the harness discovered every skill with no per-harness edit to any file; after bootstrap, the fill check over the copy's skeleton-derived artifacts comes back empty; every path reference inside the copy resolves inside the copy; the executed milestone's living sections carry output that was observed in that session, not restated from the plan; and the session stopped after one milestone rather than continuing. It also states two failing cases, one scriptable and one harness-driven, and it requires harness, version, date and outcome to be recorded in the plan that runs the trial — this file.

What exists today, for a session that has not looked: no `tools/blueprint-eval`; `docs/capabilities/index.md` lists `blueprint-eval` as `specced` with no enforcement point; `docs/DEBT.md` carries `D7` saying the blueprint has never been run and `D4` saying the procedures sit outside every harness's auto-discovery root; `GOALS.md` lists paper-only evaluation first under Known unknowns; `docs/MATURITY.md` says `blueprint-eval` needs a live trial this project has deliberately parked; and `docs/specs/bootstrap-flow.md` ends with a section stating that nobody has bootstrapped a different project from the payload and that every claim on that page is about a tree of files rather than a project that has lived with it. Each of those five statements is edited by this plan, and M7 is where they are.

## Plan of Work

Seven milestones. M1 builds what the trial needs and changes no claim. M2 and M3 run the first harness. M4 and M5 run the other two. M6 demonstrates the failing case that gates promotion and sets the status. M7 routes everything to its owner and closes the plan.

Two rules bind every milestone from M2 onward, and both come from the card. A fix for anything the trial reveals belongs in this repository — in `template/` and in the live root artifact that corresponds to it, or in `skills/`, whichever owns the defect — and never in the copy, because the copy is rebuilt by the next trial. And a payload fix that lands after a harness has already passed invalidates that harness's result: append a re-run milestone per the Decision Log rather than leaving a record that describes a payload that no longer exists.

### M1 — Make the trial runnable

At the end of this milestone the trial has a driver and the card states how its first observation is judged. Nothing has been observed in a harness, so no status changes and no debt row moves.

Acceptance, seven observable checks.

1. `./tools/blueprint-eval new smoke` prints three paths under `$HOME/blueprint-trials`, the trial directory and the `repo/` and `logs/` directories inside it; `git -C <trial>/repo log --oneline` shows exactly one commit; and `ls <trial>/repo` lists `AGENTS.md`, `ARCHITECTURE.md`, `GOALS.md`, `docs`, `plans` and `skills`.
2. `./tools/blueprint-eval check <trial>/repo layout references` on a fresh copy exits zero: the layout part reports six procedures, and the references part reports 144 of 146 references resolved with one allowlist entry applied and none stale. `./tools/blueprint-eval check <trial>/repo fill` on that same copy exits 1 and names marker lines in all eight skeleton-derived artifacts — the copy is unfilled, and a fill part that passed on it would be deciding nothing. The numbers come from the measurement in Surprises; a different count is a finding to record before continuing.
3. The card's second failing case, run as the card states it: delete `template/docs/PRINCIPLES.md`, build a fresh copy, and run the references part. It exits 1 naming `AGENTS.md`, the line number of the map entry, and `docs/PRINCIPLES.md`, and its message says the fix belongs in the payload rather than in the copy. Restore the file with `git checkout -- template/docs/PRINCIPLES.md`, rebuild, and observe the part exit zero.
4. The scriptable half of the card's first failing case: rename `skills/harness-init/SKILL.md` to `skills/harness-init/README.md`, build a fresh copy, and run the layout part. It exits 1 naming `skills/harness-init/SKILL.md` as missing and stating that the layout is fixed by every supported harness at once. Restore the name, rebuild, and observe the part exit zero.
5. `./tools/blueprint-eval check .` — this repository, not a trial copy — exits 2 and refuses, naming the destination and the reason a trial must run outside this tree.
6. `./tools/verify` still ends in `6 of 6 checks passed`, and the `doc-integrity`, `prose-duplication` and `boundary-lint` summary lines still report zero violations. The driver is not in the check list; confirm by reading `tools/verify`'s `CHECKS` line.
7. `docs/capabilities/blueprint-eval.md` states, inside its Acceptance section, how the discovery observation is judged and that the operator's prompt may not name a procedure, or a filename or path among the arrived artifacts and procedures — the target project's own intended layout is owner input and exempt. `AGENTS.md`'s Commands section names `./tools/blueprint-eval` with one line saying what it is for, and `ARCHITECTURE.md`'s Check layer entry names it as the trial driver that is not a check and is not run by the cheap command. `docs/capabilities/index.md` is untouched: the status stays `specced`.

The work. Write `tools/blueprint-eval` to the interface given under Interfaces and Dependencies. It is POSIX `sh` with `awk`, `grep`, `find`, `sed` and `test`, like every check here, because `GOALS.md` constrains this repository to markdown, git and shell. Its header comment names `docs/capabilities/blueprint-eval.md` as the card it serves and `docs/capabilities/doc-integrity.md` as the owner of the reference definition it reimplements, with the reason from the Decision Log.

Seed `tools/allow/blueprint-eval.txt` with the one key the measurement found, `docs/capabilities/doc-integrity.md:docs/NOPE.md`, and the reason that the card's own failing case names a path that must not exist. Anything else the first run reports is a genuine defect in the payload: fix it in both halves and record it in Surprises with the evidence.

Two cautions from this repository's own history, both recorded in `plans/completed/mechanical-gate-set.md` and worth repeating because they cost sessions. A remediation message that quotes a path which must not exist has to sit in an indented block or outside backticks, or `doc-integrity` reports the message itself as a broken reference. And a new sentence added to `AGENTS.md` or `ARCHITECTURE.md` that phrases itself like the card's own prose will trip `prose-duplication`'s eight-word window; write the command's one-line description in words the card does not use, and run `./tools/verify` before committing rather than after.

The card edit is a specification change, not a result: it states the criterion the Decision Log settles, so that the three harness milestones judge the same property. Do not add results to the card — it holds none, by its own last line.

### M2 — Claude Code, bootstrap half

This milestone builds the first trial repository and runs one driven session in it: the bootstrap. At the end, the copy is a filled artifact set for the `tally` project with no code in it yet, and observations 1 through 3 are recorded for Claude Code.

Acceptance, six observable checks.

1. The trial directory exists under `$HOME/blueprint-trials`, its path is written into this plan's Progress entry, and `git -C <trial>/repo log --oneline` shows the payload commit followed by the bootstrap session's own commits.
2. `./tools/blueprint-eval check <trial>/repo` exits zero with all three parts passing: the layout part finds six procedures, the fill part finds no marker anywhere in the copy, and the references part resolves every reference inside the copy. This is observations 2 and 3, and the exact summary lines go into the plan.
3. The discovery record for Claude Code is in this plan: the exact invocation line used; the harness version from `claude --version`; the date; whether any file was created or edited outside `repo/`'s own artifacts to make the procedures visible, evidenced by `ls -a <trial>/repo` showing no harness configuration directory that the copy did not arrive with; and a quoted line or two from the transcript showing how the session named the procedure it followed. This is observation 1 under the criterion M1 wrote onto the card, and it is the evidence `D4` is waiting for.
4. No prompt named this repository: `grep -rc 'harness-blueprint' <trial>/logs/` reports zero for every brief file, and the brief files are the ones this plan specifies verbatim.
5. The copy's filled `AGENTS.md` is under about 100 lines, its map has a line naming `skills/`, and its Commands section says honestly that the project has no verification command yet — there is no code at bootstrap, and `skills/harness-init/SKILL.md` forbids publishing a command nobody ran. The copy's `docs/capabilities/` keeps `fast-verify`, `isolated-env` and `boundary-lint`, each with a register row, and `docs/DEBT.md` in the copy carries at least the missing-verification-command row.
6. `./tools/verify` in this repository still passes, and `git status --porcelain` here reports nothing outside this plan file — the trial mutates nothing in this tree, and the plan's own evidence commits are the session's only change here.

The work. Build the trial repository with `./tools/blueprint-eval new claude-code`. Write the three briefs from Interfaces and Dependencies into `<trial>/logs/brief-bootstrap.md`, `<trial>/logs/brief-author.md` and `<trial>/logs/brief-execute.md`, verbatim, with no path to this repository anywhere in them; they live in `logs/` rather than in `repo/` so that they are not artifacts of the project under test. Run the bootstrap session with the invocation under Concrete Steps, capturing the transcript to `<trial>/logs/01-bootstrap.txt`. Then run the driver's three parts and read the copy.

Record what the session did with the two things authoring found. `skills/harness-init/SKILL.md`'s Locate step tells it to copy the artifacts from the checkout it was invoked from, which does not exist here; the brief tells it the copy arrived already done, and the milestone records whether that read as followable or as a contradiction the session had to work around. And the payload's map cannot name the procedures, so record how the session found them in the absence of a map line.

If the session leaves a marker in a filled artifact, a dangling reference, or an artifact it invented rather than filled, that is a finding, not a failure of the milestone: record it, decide whether it is a defect in the payload or in the procedure, and fix it where it belongs, in both halves when the defect is in the payload. If a fix is bigger than the remaining session, split the milestone in place in `Progress`, log the split, and stop.

### M3 — Claude Code, plan and execution half

Two more driven sessions in the same trial repository: one authors the target's first plan, one executes its first milestone. At the end, `tally` runs, and observations 4 and 5 are recorded for Claude Code.

Acceptance, six observable checks.

1. A plan file exists under `<trial>/repo/plans/active/`, carries all four required living sections — `Progress`, `Surprises & Discoveries`, `Decision Log`, `Outcomes & Retrospective` — and describes at least two milestones, of which the first delivers `tally add` and `tally report` end to end.
2. In `<trial>/repo`, `python3 -m tally add build && python3 -m tally add build && python3 -m tally add ship && python3 -m tally report` prints one line per label with counts, `build` above `ship`; the exact output goes into this plan. The store file the run created is inside the checkout, at `.tally/store.tsv` unless the project documented another path. Then `TALLY_STORE=$(mktemp -d)/s.tsv python3 -m tally report` reads that empty path instead and reports none of the labels above, which is what makes the store path an override rather than a default nobody can move.
3. The project's own verification command, as published in the copy's `AGENTS.md`, runs and passes; record the command and its output. Expect a `python3 -m unittest` invocation with at least one real test of the counting logic.
4. Observation 4, judged from git rather than from prose: the diff of the plan file between the authoring commit and the execution commit adds at least one line of recorded output that does not appear in the authoring version — the test summary, the `report` output, or the store path. Quote the added line and state which commit introduced it. Output restated from the plan's own expectations fails this observation, and a failure here is recorded, not smoothed over.
5. Observation 5: the plan's `Progress` shows exactly one entry ticked and timestamped, the later milestones still open, and `git -C <trial>/repo log --oneline` shows no commit implementing a later milestone's scope. If the session continued past one milestone, record it as a failure of this observation and name what it did.
6. The outcome line for Claude Code is in this plan: harness, version, date, and pass or fail for each of the five observations, with the shortfall named for any that failed.

The work. Run the authoring session, then the execution session, as two separate invocations with no resumption of the first — the card requires the execution to happen in a fresh session, and a separate process invocation with no `--continue` or `--resume` is what makes that observable. Capture transcripts to `<trial>/logs/02-author.txt` and `<trial>/logs/03-execute.txt`. Between the two, commit nothing in this repository except this plan.

The evidence discipline that binds this session binds the one under test too, and the asymmetry is worth holding in mind: this milestone's job is to observe what the driven session recorded, not to improve it. Do not edit the trial repository by hand to make an observation pass.

### M4 — omp, whole trial in one session

Everything M2 and M3 did, in one session, against a fresh trial repository built for omp. The payload has already been through one full trial, so this session's expected cost is the three driven sessions plus the evidence.

Acceptance: the same checks as M2 items 1 through 6 and M3 items 1 through 6, against a trial directory built with `./tools/blueprint-eval new omp`, with the harness version from `omp --version` and the invocation under Concrete Steps. Two additions specific to this harness. Record whether omp surfaced the procedures automatically and, if its output says so, from which root and under what configuration — `omp --help` documents `--no-skills` and `--skills=<glob>`, so the mechanism exists and what it scans is exactly the `D4` question. And record whether the session was run with `--auto-approve`; a permission flag is not a file edit and does not violate observation 1, but it is part of the invocation and belongs in the record.

If this session finds a payload defect, the Decision Log's re-run rule applies: fix it in this repository, and append a re-run milestone for Claude Code, whose result the fix invalidates.

### M5 — Codex CLI, whole trial in one session

The same again for Codex CLI, whose help text names no skills mechanism at all, which makes it the harness where the map is the only route to a procedure.

Acceptance: the same checks as M4, against a trial directory built with `./tools/blueprint-eval new codex`, with the harness version from `codex --version` and the invocation under Concrete Steps. Three additions. Record how the session reached the bootstrap procedure with no skills mechanism and no map line naming it, quoting the transcript. Record the sandbox and approval flags used; `codex exec` defaults to a sandbox that denies network access, which the dependency-free target does not need, so a run that needed more permission than `-s workspace-write --approve-for-me` is itself an observation worth the line it takes. And state explicitly whether observation 1 held under the criterion on the card, because this is the harness that criterion was written for.

### M6 — The discovery failing case, and the status

The card gates promotion on a failing case in a harness, not only in the driver: rename the bootstrap procedure's file, run one harness against a fresh copy, and observe it fail naming the harness and the skill it could not find. This milestone does that and then sets the status to whatever the four preceding milestones observed.

Acceptance, five observable checks.

1. With `skills/harness-init/SKILL.md` renamed to `skills/harness-init/README.md` in this repository and a fresh trial repository built from that state, the bootstrap brief run in the harness that showed the strongest discovery in M2 through M5 does not bootstrap: it reports that it cannot find the procedure the task needs. Quote what it reported, and name the harness and its version. A session that bootstrapped anyway — by reading `README.md` and following it, or by inventing the artifacts — is itself the finding, and it is recorded as one: it means the layout half of the invariant is not load-bearing in that harness, which is a fact the card's failing case exists to discover.
2. `./tools/blueprint-eval check <trial>/repo layout` on that same copy exits 1 naming `skills/harness-init/SKILL.md`, which is the driver's half of the same case, re-observed here against a copy that a harness also saw.
3. The rename is undone with `git mv` and `./tools/verify` passes again; `ls skills/harness-init/` shows `SKILL.md` and no `README.md`. The allowlist entry in `tools/allow/doc-integrity.txt` that names `skills/harness-init/README.md` is untouched throughout — it is the card's mention of the file, not a reference to the file, and removing it would make `doc-integrity` report the card.
4. `docs/capabilities/index.md` records `blueprint-eval` as `built` with `./tools/blueprint-eval` plus the trial procedure named in the enforcement column, if and only if all five observations held in all three harnesses and both failing cases were demonstrated. Otherwise it stays `specced` and this plan records which observation in which harness withheld the promotion. The status lives only in that file.
5. `./tools/verify` passes and `docs/capabilities/blueprint-eval.md` still holds no results.

The work. This is the promotion bar from `docs/capabilities/CARD_FORMAT.md` — a check nobody has seen fail is not known to check anything — applied to a check that is three quarters procedure. Which harness to use is decided by what M2 through M5 observed: pick the one whose discovery was most automatic, because that is the one where a missing file has the furthest to fall. Record the choice and its reason.

The status decision is the judgement this milestone owes. Three passing trials and two demonstrated failing cases earn `built`. Anything less does not, and the honest record is a card that stays `specced` with the shortfall written here, which is the outcome `docs/capabilities/blueprint-eval.md` itself describes when it says that adding a harness to the supported list without running the trial claims a result nobody observed.

### M7 — Close the plan out

Route every finding to its owning file, delete what the trial retired, and leave the repository stating what is now true.

Acceptance, eight observable checks.

1. `docs/DEBT.md` no longer contains a `D7` row, and no `D7` section exists to delete — that row never had one. Confirm with `grep -n 'D7' docs/DEBT.md` printing nothing.
2. `D4`'s row and its Details section carry the observed discovery facts from M2 through M5 as facts — which harness surfaced the procedures automatically, from which root, and which reached them only through the map — replacing the reasoning that stands there now. Neither the row nor the section references a plan file: `ARCHITECTURE.md`'s `no-plan-file-dependency` rule forbids it, and `tools/checks/boundary-lint` decides it. `D4`'s trigger is rewritten to name the decision that is now due rather than the trial that has now run.
3. `GOALS.md`'s Known unknowns no longer says evaluation is paper-only; it states what the trial observed, in one or two lines, and `template/GOALS.md`'s corresponding skeleton section is checked for whether the same edit is owed there — the payload's copy is a skeleton, so the usual answer is no, and the check is recorded either way.
4. `docs/MATURITY.md`'s Current rung section states `blueprint-eval`'s new status and, if it reached `built`, why it cannot reach `enforced` and what that means for its L2 row.
5. `docs/specs/bootstrap-flow.md`'s last section no longer says nobody has bootstrapped a different project; it states what was observed, in which harnesses, and on what date, and what remains unobserved. `docs/specs/mechanical-checks.md`'s first section acknowledges the second runnable command and states that it decides the scriptable part of the trial while the rest is observed by a person. `docs/specs/index.md`'s rows still describe what those files cover.
6. The durable decisions from this plan's log are graduated into `docs/decisions/` as `0022` onwards, in the format `docs/decisions/DECISION_FORMAT.md` defines. The candidates, to be judged at close-out rather than promised now: the trial-target choice and the reason a document system was refused; the discovery criterion the card now carries; the ceiling on the card's status; and the re-run rule for payload fixes mid-trial.
7. `./tools/verify` passes, ending in `6 of 6 checks passed`, with the evidence check reporting no active plans once this file has moved.
8. `Outcomes & Retrospective` above is written, and this file is at `plans/completed/blueprint-live-trial.md`.

The work. `plans/PLANS.md`'s lifecycle section governs the order: write the retrospective, reflect the behavioral outcome into `docs/specs/`, graduate the durable decisions, then move the file. The one trap is the reference rule in item 2: this plan is the only place the trial's evidence lives, and nothing outside `plans/` may point at it, so every artifact that needs a trial fact states the fact rather than citing where it was observed.

One finding from authoring is routed here because this is the milestone that has `docs/DEBT.md` open anyway: the empty count in `tools/checks/evidence-check`'s summary line, recorded in Surprises. Decide it rather than carry it — either the one-line fix that initialises the counter, which is a cosmetic change to a check whose decision is unaffected and needs no failing-case demonstration, or a debt row saying why it was left. Record which, and do not let it widen into a pass over that check.

## Concrete Steps

Every command below runs from this repository's root unless a working directory is named. `<trial>` stands for the path `./tools/blueprint-eval new` printed, which the milestone's `Progress` entry records so the next session can find it. Expected transcripts are labelled expected: nothing here has been run.

Build a trial repository and check it.

    ./tools/blueprint-eval new claude-code
    # expected:
    # blueprint-eval: trial at /Users/<you>/blueprint-trials/claude-code-20260917-143000
    # blueprint-eval:   repo  /Users/<you>/blueprint-trials/claude-code-20260917-143000/repo
    # blueprint-eval:   logs  /Users/<you>/blueprint-trials/claude-code-20260917-143000/logs
    # blueprint-eval: copied 25 files — 19 payload files and 6 procedures — committed as "Receive the payload".

    ./tools/blueprint-eval check <trial>/repo
    # expected on a fresh, unfilled copy: layout and references pass, fill fails.
    # blueprint-eval layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.
    # blueprint-eval fill: AGENTS.md:1 still carries authoring scaffolding.
    #   …
    # blueprint-eval fill: 117 marker lines in 8 of 23 markdown files.
    # blueprint-eval references: ok — 144 of 146 references resolved inside the copy, 1 allowlist entry applied, 0 stale.

Run the three driven sessions. Each is a separate process with no resumption, which is what makes "a fresh session" observable. The brief files are written from this plan before the first one runs.

    cd <trial>/repo && claude -p --dangerously-skip-permissions "$(cat ../logs/brief-bootstrap.md)" 2>&1 | tee ../logs/01-bootstrap.txt
    cd <trial>/repo && claude -p --dangerously-skip-permissions "$(cat ../logs/brief-author.md)"    2>&1 | tee ../logs/02-author.txt
    cd <trial>/repo && claude -p --dangerously-skip-permissions "$(cat ../logs/brief-execute.md)"   2>&1 | tee ../logs/03-execute.txt

    omp -p --auto-approve --cwd <trial>/repo "$(cat <trial>/logs/brief-bootstrap.md)" 2>&1 | tee <trial>/logs/01-bootstrap.txt
    # and the same shape for the authoring and execution briefs.

    codex exec -C <trial>/repo -s workspace-write --approve-for-me "$(cat <trial>/logs/brief-bootstrap.md)" 2>&1 | tee <trial>/logs/01-bootstrap.txt
    # and the same shape for the authoring and execution briefs.

If a harness refuses to run non-interactively, cannot authenticate, or stops on a prompt no flag answers, that is a finding: record what it printed, then run that harness's sessions interactively in the same trial repository with the same briefs pasted in, and say in the record that the session was driven by hand. If it cannot be driven at all, the trial is incomplete for that harness, the card stays `specced`, and M6 records which harness withheld the promotion. Do not substitute a different harness.

Read the result.

    ./tools/blueprint-eval check <trial>/repo
    cd <trial>/repo && git log --oneline
    cd <trial>/repo && python3 -m tally add build && python3 -m tally add build && python3 -m tally add ship && python3 -m tally report
    cd <trial>/repo && grep -n '^## ' plans/active/*.md
    cd <trial>/repo && git show --stat HEAD
    ls -a <trial>/repo
    grep -rc 'harness-blueprint' <trial>/logs/

Demonstrate the failing cases (M1 for the driver, M6 for the harness), and restore immediately in both directions.

    rm template/docs/PRINCIPLES.md
    ./tools/blueprint-eval new dangling-map && ./tools/blueprint-eval check <trial>/repo references
    git checkout -- template/docs/PRINCIPLES.md && ./tools/verify

    git mv skills/harness-init/SKILL.md skills/harness-init/README.md
    ./tools/blueprint-eval new missing-skill && ./tools/blueprint-eval check <trial>/repo layout
    git mv skills/harness-init/README.md skills/harness-init/SKILL.md && ./tools/verify

## Validation and Acceptance

The plan as a whole is accepted when a reader who has just cloned this repository can do four things. Run `./tools/blueprint-eval new scratch` and `./tools/blueprint-eval check <trial>/repo`, and watch three named parts decide the layout, the scaffolding and the references of a copy of the payload. Read in this file, for each of `claude`, `omp` and `codex`, the harness's version, the date, the invocation, the outcome of each of the card's five observations, and how that harness found the procedures it followed. Read in `docs/capabilities/index.md` a status for `blueprint-eval` that matches those records, with the failing cases that justify it transcribed here. And find no `D7` in `docs/DEBT.md`, a `D4` that carries observations instead of reasoning, and a `docs/specs/bootstrap-flow.md` that no longer says the payload has never left this tree.

Per-milestone acceptance is stated in each milestone above, and a session records the command it ran and what it printed, not what this plan expected. An acceptance check that could not be run is an unmet acceptance: name the gap against the clause it belongs to and leave the entry honest. The one acceptance this plan cannot state as a command is observation 4 — whether the driven session's recorded evidence was observed rather than restated — and M3 pins it to the narrowest observable form available: a line of output in the plan's execution commit that is absent from its authoring commit.

## Idempotence and Recovery

`./tools/blueprint-eval new` is safe to run repeatedly: each invocation makes a new timestamped directory and refuses to write into one that exists, so a trial that went wrong is abandoned rather than repaired, and the next one starts from a clean copy. `check` only reads. Nothing either subcommand does touches this repository's tracked files.

The two failing-case demonstrations are the only steps that mutate this repository, and both are one command to undo — `git checkout --` for the deleted file, `git mv` for the rename. Run `./tools/verify` immediately after each restoration and before committing anything else; a session that dies between the mutation and the restoration leaves a tree where `template-live-drift` or the layout part fails, and the recovery is `git status` followed by the inverse command.

Trial directories under `$HOME/blueprint-trials` are scratch and accumulate. Leave a harness's directory in place until that harness's milestones are all recorded — M3 works in the repository M2 built — and delete old ones with `rm -rf` once their evidence is in this file. Nothing in a trial directory is ever copied back into this repository.

If a driven session leaves the trial repository half-bootstrapped, do not hand-finish it. Record what it did and why the milestone is incomplete, and start a fresh trial directory for the retry, so that the record of the retry is a record of the payload and not of the repair.

## Artifacts and Notes

Transcripts live at `<trial>/logs/01-bootstrap.txt`, `02-author.txt` and `03-execute.txt`, beside the briefs. They are outside this repository and they are not durable: the path is recorded here as provenance, and the lines that prove an observation are quoted in this plan, short and labelled, because a path into a scratch directory proves nothing to a reader six months from now.

What gets quoted, per harness: the driver's three summary lines after bootstrap; the one or two transcript lines showing how the session named the procedure it followed; the `report` output and the verification command's output from the executed milestone; and the added line from the plan-file diff that shows the execution session wrote something it observed. Everything else stays in the transcript.

## Interfaces and Dependencies

### The driver: `tools/blueprint-eval`

POSIX `sh`, executable, at `tools/blueprint-eval` — directly under `tools/`, not under `tools/checks/`, and absent from `tools/verify`'s `CHECKS` list. Two subcommands.

`new <label>` builds a trial repository and prints where. It creates `${BLUEPRINT_TRIAL_ROOT:-$HOME/blueprint-trials}/<label>-<YYYYmmdd-HHMMSS>/` holding `repo/` and `logs/`; copies the payload with `cp -R template/. repo/` so that the directory placeholders come too; copies the procedures with `cp -R skills repo/skills`; runs `git init`, `git add -A` and one commit reading `Receive the payload`; and prints the trial path, the two paths inside it, and a count of the payload files and procedures copied. It exits 2 without writing anything when the destination already exists, when the resolved destination is inside this repository's worktree, or when `template/` or `skills/` is missing from this checkout.

`check <repo-dir> [layout] [fill] [references]` decides the scriptable parts, all three when none is named. It exits 0 when every named part holds, 1 on a violation, 2 when it cannot decide — a missing directory, an unreadable copy, or a `<repo-dir>` inside this repository. Each part prints one summary line on success naming what it examined and how much of it, and one block per violation naming the part, the file, the line and the next action, with the card's rule that a fix belongs in this repository's payload and never in the copy.

The `layout` part reads the procedure names from this checkout's own `skills/` directories and requires each one to exist in the copy at `skills/<name>/SKILL.md`, with frontmatter carrying a `name` equal to the directory name and a non-empty `description`. A directory present with no `SKILL.md`, a `name` that disagrees with its directory, or a procedure missing from the copy altogether is a violation naming the expected path.

The `fill` part greps every `*.md` under `<repo-dir>` for the two marker shapes `template/AGENTS.md` defines and requires no match. Its own pattern is written with the final letter of each marker word in brackets, the trick `tools/checks/scaffolding-markers` uses, so that the driver cannot match itself; `docs/DEBT.md` `D2` holds the known limit of matching the words anywhere.

The `references` part extracts, from every `*.md` under `<repo-dir>` except files whose own name ends in `_FORMAT.md` and files under `plans/active/` and `plans/completed/`, every backticked span and markdown link target containing a slash and none of space, tab, asterisk, angle bracket, brace or colon, ignoring fenced and indented blocks as quoted material. The convention document at `plans/PLANS.md` stays in scope — it is not a plan in flight — which matches the checked set `tools/checks/doc-integrity` uses. A reference resolves when it exists relative to `<repo-dir>` or relative to the citing file's own directory, and nowhere else; there is no parent-directory allowance, for the reason in the Decision Log. Deliberate exceptions live in `tools/allow/blueprint-eval.txt`, one per line as the citing path, a colon, the reference, two spaces, a `#` and a one-line reason, and the summary line reports how many applied and how many matched nothing. `docs/capabilities/doc-integrity.md` owns the reference definition; the driver reimplements it for a different root and a different checked set, and says so in its header.

### The trial target: `tally`

Identical in all three trials. A single-user command-line tool that records labels and prints counts, with no third-party dependency, no network, no server and no installation step.

    tally/__main__.py      argv entry point: python3 -m tally
    tally/cli.py           argument parsing, stdout
    tally/store.py         reads and writes the store file
    tally/core/            pure functions over strings and records
    tests/                 standard-library unittest tests

The layering rule the project declares, and the one `boundary-lint` decides for it: nothing under `tally/core/` may import `tally.store` or `tally.cli`. The store path is `.tally/store.tsv` inside the checkout, overridden by the `TALLY_STORE` environment variable. The commands the project ends the trial with: `python3 -m unittest discover -s tests -t . -q` to verify, `python3 -m tally add <label>` and `python3 -m tally report` to run. Nothing is pinned about how the store is formatted or how ties are ordered; those are the target project's own decisions to make and record.

### Brief one: bootstrap

Written verbatim to `<trial>/logs/brief-bootstrap.md`. It names no procedure and no filename or path among the arrived artifacts, because the discovery observation depends on the session finding those itself; the `tally` layout it does name is owner interview input, exempt under the criterion's own terms.

    You are working in a git repository that has just received an agent-harness
    payload: skeleton knowledge artifacts and a set of portable procedures are
    already committed here as the first commit. Nothing outside this repository
    is available to you, and you must not go looking for it.

    Task: bootstrap this repository's agent harness, following the repository's
    own procedures, and then stop. Do not start the project's first feature.

    The copy step is already done. The artifacts and the procedures arrived by a
    plain recursive copy and are committed. Treat any instruction to copy them
    in from somewhere else as already satisfied, do not re-copy anything, and do
    not overwrite anything that is already here.

    Owner answers to the questions no file in this tree can answer:

    Project name: tally.

    What it is: a command-line tool that keeps a running count of labels. You
    type "tally add deploy" when something happens, and "tally report" to see
    how often each label has happened.

    Who it serves: one developer, on one machine, from a terminal. There is no
    server, no network use, and no second consumer.

    Outcomes that would count as success: recording a label takes one short
    command and no setup; the counts can be read back in one command; and two
    checkouts of this project on the same machine never share or corrupt each
    other's stored data.

    Deliberately out of scope: multiple users, syncing between machines, date
    or time-range filtering, editing or deleting recorded entries, and any
    output format other than plain text.

    Non-goals worth writing down because somebody will propose them: no
    third-party dependencies, ever — the standard library only; no network
    access; no database server; no background process; no configuration file.

    Constraints that outrank convenience: the stored data lives inside the
    checkout unless the TALLY_STORE environment variable points elsewhere; the
    counting logic stays free of file and terminal input/output so it can be
    tested without either; and the tool runs on the system python3 with no
    installation step.

    Intended layering: tally/core/ holds pure functions; tally/store.py reads
    and writes the stored data; tally/cli.py parses arguments and prints.
    Nothing under tally/core/ may import tally.store or tally.cli.

    Questions the owners know they cannot answer yet: whether the stored format
    survives labels containing tabs or newlines, and whether a single file stays
    fast enough past a few thousand entries.

    Commands that exist today: none. There is no code in this repository yet,
    and no test command. The first plan is what establishes one.

    Capability register: keep the cards for the cheap verification command, for
    environment isolation, and for the dependency-direction check. This project
    has a real toolchain, a real state path and a real layering rule, so all
    three decide something here. Judge the rest yourself and record the reason
    for each one you leave out.

    When you are done, report what you created, what you left out and why, and
    what you deferred. Then stop.

### Brief two: plan authoring

Written verbatim to `<trial>/logs/brief-author.md`.

    You are working in a git repository whose agent harness is already
    installed: read its own guide first and follow its conventions. Nothing
    outside this repository is available to you, and you must not go looking
    for it.

    Task: author a plan for this project's first feature, following the
    repository's own conventions and its own authoring procedure, and stop at a
    committed plan. Do not implement any of it.

    The feature: the first working version of tally. "python3 -m tally add
    <label>" records a label; "python3 -m tally report" prints one line per
    label with its count, most frequent first. Stored data lives in a file
    inside the checkout, unless the TALLY_STORE environment variable names
    another path. Standard library only, and the project's cheap verification
    command must exist and be published in the repository's guide by the end of
    the first milestone.

    Cut the work so that the first milestone delivers add and report end to end
    with a test that fails before it and passes after, and later milestones
    carry whatever you judge comes next. Do not put the whole feature in one
    milestone.

### Brief three: execution

Written verbatim to `<trial>/logs/brief-execute.md`.

    You are working in a git repository whose agent harness is already
    installed, and which has a plan in flight. Nothing outside this repository
    is available to you, and you must not go looking for it.

    Task: execute the first unfinished milestone of that plan, following the
    repository's own execution procedure, and then stop.

### What this plan deliberately leaves out

Building the target's three cards into running checks: the trial instantiates `fast-verify`, `isolated-env` and `boundary-lint` for a project that can violate them, which is the first exercise those cards have had, but promoting any of them to `built` needs its own demonstrated failing case and is the target project's work, not this trial's.

Deciding where the procedures should live. `D4` has three candidate answers and this plan records evidence for all three instead of picking one; the pick is the next plan's, and it changes what the payload installs, which makes it a decision rather than a fix.

Mechanizing the harness-driven three quarters of the card. `docs/capabilities/blueprint-eval.md` already says mechanization is bounded by the harnesses themselves, and this plan does not test that boundary: it scripts what the card lists as scriptable and drives the rest with the transcripts recorded.

A second trial target, a brownfield target, and any judgement about how the payload lands on an existing codebase. `docs/DEBT.md` `D6` owns the brownfield gap and nothing here touches it.

Anything about `loop-runner`, `D5`, or the rung above L0. The trial produces evidence about the payload, not about unattended iteration, and `docs/MATURITY.md`'s promotion rule needs twenty consecutive green landed changes and an unskippable gate that this plan does not provide.

## Revision Notes

- 2026-09-17 (pre-execution review): scoped the discovery criterion — in the
  Decision Log, M1 acceptance 7, and the brief-one preamble — to filenames
  and paths *among the arrived artifacts and procedures*, exempting the
  target project's own intended layout as owner interview input. Reason:
  the unqualified criterion contradicted brief one, which names the tally
  module layout; a strict judge would fail observation 1 against the brief
  itself and a loose one would judge differently per harness, which is the
  divergence the criterion decision exists to prevent. No milestone
  boundaries, acceptance counts, or contracts changed.
