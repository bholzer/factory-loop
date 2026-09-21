# Run the live trial the blueprint has never had: drive one feature through the payload in Claude Code, omp, and Codex CLI

This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

This document must be maintained in accordance with `plans/PLANS.md`.

## Purpose / Big Picture

Every claim this repository makes about the payload in `template/` and the procedures in `skills/` has been established by reading them from inside the repository that produced them. Six mechanical checks decide structure, correspondence, reference direction and evidence discipline, and all six read the payload from a tree where a dangling reference still resolves against an ancestor and an unstated assumption is still in an author's head. Nobody has ever copied the payload into an empty repository, handed it to an agent with no access to this tree, and watched a feature come out the other end. `docs/DEBT.md` records that as `D7`, `docs/capabilities/blueprint-eval.md` specifies the trial that settles it, and `GOALS.md` lists it first under Known unknowns.

After this plan, an outside reader can see three things that do not exist today. First, a command in this repository, `./tools/blueprint-eval`, that builds a trial repository outside this tree from the payload and the procedures and then decides the three parts of the trial a machine can decide: that the procedures arrived in the layout every supported harness requires, that no authoring scaffolding survived the bootstrap, and that every path reference inside the copy resolves inside the copy. Second, a record in this plan of one real feature — a small command-line tool called `tally`, with a runtime surface a person can exercise from a terminal — carried from bootstrap through plan authoring to an executed milestone, once in each of the three harnesses `GOALS.md` names, with the harness, its version, the date, and the outcome of each of the card's five observations written down as observed. Third, a status for `blueprint-eval` in `docs/capabilities/index.md` that reflects what was observed rather than what was hoped: `built` if the three trials passed and the card's two failing cases were demonstrated, `specced` with the shortfall named if they did not.

Two debt items are touched, one paid and one made actionable. `D7` is paid by running the trial and is deleted when it is. `D4` — the installed procedures sit outside every harness's auto-discovery root — is not decided here; its row names this trial as its trigger, so each harness session in this plan records how it found the procedures it followed, and `D4` is rewritten at close-out to carry those observations instead of the reasoning it carries now. Choosing between the three ways of paying `D4` down is the next plan's work, not this one's.

The observation that makes this plan worth its cost is the one nobody can make from inside this tree: whether a target project that receives a plain recursive copy and nothing else can actually be worked in. The trial target is deliberately a project with a toolchain — a Python package, a test command, a state file, a layering rule — because the payload's toolchain-facing cards, `fast-verify`, `isolated-env` and `boundary-lint`, have never been instantiated anywhere. In this repository they were pruned or filled against a tree of markdown; `GOALS.md` records under Scope that `isolated-env` was not instantiated here at all, because nothing in a repository of documents has a runtime to isolate. The trial is the first time those three cards meet a project that can violate them.

## Progress

- [x] (2026-09-17 20:19Z) M1 — Make the trial runnable: `tools/blueprint-eval` built with its `new` and `check` subcommands, both scriptable failing cases demonstrated, the discovery criterion stated on the card, and the command published in `AGENTS.md` and `ARCHITECTURE.md`. All seven acceptance checks observed; commits `7361958` (driver and allowlist) and `1187484` (card, map, layer entry). Trial directories used and left in place for inspection: `smoke-20260917-151702`, `dangling-map-20260917-151721`, `dangling-restored-20260917-151726`, `missing-skill-20260917-151730`, `layout-restored-20260917-151735`, all under `$HOME/blueprint-trials`.
- [x] (2026-09-17 20:45Z) M2 — Claude Code, bootstrap half: trial at `$HOME/blueprint-trials/claude-code-20260917-152930`, harness `2.1.274 (Claude Code)`, one driven bootstrap session, commits `b55f463` (payload) and `ae00403` (the session's own bootstrap). Observation 1 held and its discovery route is recorded below; observation 2 held — `fill: ok — no authoring scaffolding in 23 markdown files of the copy.` Carve-out against acceptance 2: the references part exited 1 with 33 dangling references, all of them the target's own reserved code paths (32) plus one absolute system path (1), so observation 3 is **not** met at bootstrap and is re-observed in M3 once the executed milestone has created `tally/core/`, `tally/store.py` and `tally/cli.py`; the Decision Log entry on judging that observation at the end of a trial states why, and M8 below carries the payload fix the finding earned. Second carve-out: the bootstrap session ran with `--model opus` because this machine's configured default model refused for billing reasons.
- [x] (2026-09-17 21:57Z) M3 — Claude Code, plan and execution half: two more driven sessions in `$HOME/blueprint-trials/claude-code-20260917-152930`, authoring at 8 minutes 3 seconds (commit `c82847f`, 881 lines of plan and nothing else) and execution at 4 minutes 35 seconds (commits `fa2d92f`, `2f3ba4b`, `a3df528`, `0110215`, all prefixed `M1:`), each a separate process with no resumption and each exiting 0. `tally` runs: `add build` twice, `add ship`, `report` printed `build 2` then `ship 1`, exit 0, and the copy's own published `./verify` printed `verify: 1 check passed (tests).` over seven passing tests in 0.359 seconds. Acceptance checks 1 through 6 observed; observations 4 and 5 held, the latter judged against milestone entries by the decision logged below. Carve-out against acceptance 7: the references part still exits 1, `references: 1 dangling reference in 161 examined across 21 markdown files of the copy.`, down from 33 — the survivor is `/tmp` cited by the copy's `docs/PRINCIPLES.md:17`, which is the reference definition's defect and not the target's, so observation 3 is **not** met for Claude Code and its fix moved into M8 as that milestone's fifth acceptance check. The full outcome record for this harness is under `Outcomes & Retrospective`.
- [x] (2026-09-17 22:35Z) M4 — omp, whole trial in one session: trial at `$HOME/blueprint-trials/omp-20260917-170102`, harness `omp/18.1.14`, three driven sessions with standard input closed and no resumption, each exiting 0 — bootstrap 389 seconds (commit `47505cb`), authoring 794 seconds (commit `4a3fb86`, 931 lines of plan and nothing else), execution 554 seconds (commits `5bace42`, `89669e5`, `01161c3`, `2d9c43f`, all prefixed `M1:`) — plus two read-only probe sessions that decide the discovery question, per the decision logged below. The invocation is `omp -p --auto-approve --cwd <trial>/repo` and the permission flag is recorded here rather than counted against observation 1, as the card requires. `tally` runs: `add build` twice, `add ship`, `report` printed `build 2` then `ship 1`, exit 0, store at `.tally-store` inside the checkout and `TALLY_STORE` overriding it; the copy's own published `./check` printed `Ran 36 tests in 0.334s`, `OK`, exit 0. Observations 1, 2, 4 and 5 hold, with the omp discovery record and the outcome record below. Carve-out against the references clause M4 inherits from M2 acceptance 2 and M3 acceptance 7: observation 3 is **not** met — the part exited 1 with one dangling reference after the executed milestone, the bare core directory cited by the copy's `AGENTS.md:16`, which the decision below routes to the target project rather than to this repository, and which the fix M8 lands does not clear. This bootstrap record was made against the pre-fix `skills/harness-init/SKILL.md`, so omp joins Claude Code in M8's fourth check.
- [x] (2026-09-21 21:20Z) M5 — Codex CLI, whole trial in one session: fresh post-fix trial at `$HOME/blueprint-trials/codex-20260921-154505`, harness `codex-cli 0.150.1`, model `gpt-5.6-sol`, three driven sessions with standard input closed and no resumption, each exiting 0 — bootstrap 650 seconds (commit `60ebbe6`), authoring 595 seconds (commit `74d18e5`, 232 lines of plan and nothing else), execution 561 seconds (commit `bb405f3`) — plus the two read-only probe sessions that decide the discovery question. The invocation is `codex exec -C <trial>/repo -m gpt-5.6-sol --approve-for-me`; the approval flag is the permission flag and implies the workspace-write sandbox, and the model flag is required because the configured default is still refused for this CLI version, both recorded with the invocation as the card requires. `tally` runs: `add build` twice, `add ship`, `report` printed `build` and `ship` with tab-separated counts 2 and 1, exit 0, store at `.tally.json` inside the checkout with `TALLY_STORE` overriding it; the copy's published `python3 scripts/verify.py` printed `Ran 1 test in 0.096s`, `OK` and `fast-verify: PASS (unittest)`, exit 0 in 0.359 seconds. All five observations hold, including observation 3 — `references: ok — 108 of 110 references resolved inside the copy, 1 allowlist entry applied, 0 stale.`, all three parts exiting 0 together, the only clean walk in the trial. The stale pre-fix trial M5's first session left at `codex-20260917-174227` was abandoned rather than reused, so this harness owes no re-run. Two findings routed rather than fixed: the install step's second clause produced nothing in this harness, which is `D4` evidence, and the harness's three non-payload refusals, which belong to this machine and this account.
- [x] (2026-09-21 21:40Z) M6 — The discovery failing case, and the status: the case was run in Claude Code, harness `2.1.274 (Claude Code)`, against a fresh trial built with `skills/harness-init/SKILL.md` renamed to `skills/harness-init/README.md`, at `$HOME/blueprint-trials/missing-skill-claude-20260921-163125`. Carve-out against acceptance 1: the harness did not fail. The session read the renamed file out of the tree in its second tool call and bootstrapped anyway — 457 seconds, exit 0, commit `5e4401c` over `d161b01`, a filled artifact set with no scaffolding left in it — so what the rename produced is the finding that clause anticipates rather than the failure it expects, and the layout half of the invariant is carried by the driver alone in any harness that reads the tree. Acceptance 2 observed: the layout part exited 1 with `skills/harness-init/SKILL.md is missing from the copy.`, `found skills/harness-init/README.md instead.` and the summary `the copy carries 5 of 6 procedures in the layout every supported harness requires.` Acceptance 3 observed: the rename undone with `git mv`, `ls skills/harness-init/` showing `SKILL.md` and nothing beside it, `./tools/verify` back to `6 of 6 checks passed` with `doc-integrity: ok — 372 of 375 references resolved in 47 artifacts, 2 format documents skipped, 3 allowlist entries applied, 0 stale.`, and a fresh copy back to `layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.` Acceptances 4 and 5: the status stays `specced` and `docs/capabilities/index.md` is untouched, because the promotion condition is a conjunction and neither half of it holds — observation 3 failed in Claude Code and in omp, and the harness failing case did not fail — while the card still holds no results.
- [x] (2026-09-21 20:45Z) M8 — Land the payload fix the trial earned: `skills/harness-init/SKILL.md` step 10 now names the reserved-path class and the debt row that carries it, and its stop condition asks for the walk's result recorded rather than clean (commit `d66b272`); the reference definition no longer treats an absolute path as a repository-relative reference, stated in both halves of `docs/capabilities/doc-integrity.md` and implemented in `tools/checks/doc-integrity` and `tools/blueprint-eval` (commit `9ad06cc`). All five acceptance checks observed. `./tools/verify` ends in `6 of 6 checks passed` with `prose-duplication: ok — 45 artifacts compared, 33038 eight-word windows examined, 20 allowlist entries applied, 0 stale.` and no new allowlist entry in any file. The post-fix Claude Code bootstrap at `$HOME/blueprint-trials/claude-code-postfix-20260921-152808` (4m55s, exit 0, commit `2b2691b`) leaves 20 dangling references, every one a reserved `tally/` path, and a `docs/DEBT.md` `D2` row naming them with seven allowlist keys; the omp bootstrap re-run at `$HOME/blueprint-trials/omp-postfix-20260921-152834` (5m26s, exit 0, commit `e88f00c`) leaves 23 of the same class with eight keys, so both records made against the pre-fix text are re-run rather than labelled. The exclusion itself is demonstrated by a controlled pair over one identical copy: 147 examined with one `/tmp` citation dangling under the pre-fix driver, 146 examined with none under the committed one, allowlist untouched. Carve-out against check 4: only the bootstrap halves were re-driven, since the fix touches neither the authoring nor the execution procedure, so observations 3, 4 and 5 in both harness records stand as recorded against the pre-fix trials and no post-fix trial has yet reached an executed milestone.
- [ ] M7 — Close out: delete `D7`, rewrite `D4` with the observed discovery facts, update `GOALS.md`, `docs/MATURITY.md` and `docs/specs/`, graduate the durable decisions, and move this file to `plans/completed/`.

Use timestamps to measure rates of progress. M1 through M6 and M8 are executed; M7 alone is open, and it runs last whatever number stands in front of it. All three harnesses have now been through the whole trial. Two ended one dangling reference short of observation 3, for different reasons — the reference definition in one, the target project's own map line in the other — and the third ended clean, with the difference lying in what the target project did with its reserved paths rather than in anything the payload withheld. The two earlier harnesses had their bootstrap halves re-driven against the fixed payload by M8; the third trial was built after the fix, which is why its record needs no such note. M6 settled its harness pick on the route the records distinguish rather than on a skills mechanism none of the three used, and the rename it ran under changed nothing: the session read the renamed file straight out of the tree and bootstrapped, which is why the card is where M6 left it. M8 was appended by M2's session, which found a payload defect whose fix invalidates a bootstrap record, and it sat before M7 because close-out is the last thing that happens and a fix landing after it would close a plan over a payload nobody re-trialled; it keeps the number 8 because M1 through M7 were already spent in this record. M3's session added a fifth acceptance check to it rather than a ninth milestone, for the reason its decision entry gives. The session that executes the one open milestone adds the observation time to its entry in the shape the entries above use, and records its carve-outs against the acceptance clause each one departs from.

## Surprises & Discoveries

Everything below was observed by running the command named in each entry, never predicted. The entries down to the empty-count one were observed while authoring this plan, on 2026-09-17; the entries below the marker line were observed by the session that executed M1, the same day.

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

Observed during M1 execution, 2026-09-17, in commits `7361958` and `1187484`. Everything above the marker predates the driver.

- Observation: the driver's counts came out identical to the numbers the authoring measurement predicted, in every one of the three parts, on the first run.
  Evidence: `./tools/blueprint-eval new smoke` printed `copied 25 files — 19 payload files and 6 procedures — committed as "Receive the payload".`; `check <trial>/repo layout references` printed `layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.` and `references: ok — 144 of 146 references resolved inside the copy, 1 allowlist entry applied, 0 stale.`; `check <trial>/repo fill` exited 1 and ended `fill: 117 marker lines in 8 of 23 markdown files.` The eight files it named are exactly the eight skeleton-derived artifacts — `AGENTS.md`, `ARCHITECTURE.md`, `GOALS.md`, `docs/DEBT.md`, `docs/MATURITY.md`, `docs/PRINCIPLES.md`, `docs/capabilities/index.md` and `docs/specs/index.md` — the same set `tools/checks/scaffolding-markers` scans in this repository, reached in the copy by a walk rather than by a list. Running all three parts in one invocation exits 1 and prints 128 lines in the order layout, fill, references. The seeded allowlist entry was the only one needed: nothing else in a fresh copy dangles, so the payload half of the trial starts from a clean copy rather than from a backlog.

- Observation: one deleted payload file produces ten dangling references, not one, and the map entry M1's acceptance names is only the first of them. The failing case is therefore stronger than the acceptance clause that describes it.
  Evidence: with `template/docs/PRINCIPLES.md` deleted, `check <trial>/repo references` exited 1 with ten blocks — `AGENTS.md:49`, `GOALS.md:90`, `docs/MATURITY.md:58`, `docs/capabilities/prose-duplication.md:30` and `:79`, `skills/doc-garden/SKILL.md:50`, `skills/harness-init/SKILL.md:90`, `skills/plan-author/SKILL.md:38`, `skills/retro/SKILL.md:36` and `:88` — and the summary `references: 10 dangling references in 143 examined across 20 markdown files of the copy.` The same run's `new` line read `copied 24 files — 18 payload files and 6 procedures`, so the count line reports the deletion too. After `git checkout -- template/docs/PRINCIPLES.md` and a fresh copy, the part printed `ok — 144 of 146` again.

- Observation: the reference definition `docs/capabilities/doc-integrity.md` owns treats a backticked path rooted in an environment variable as a path to resolve, so a `AGENTS.md` line naming the default trial root in backticks is reported as a broken reference.
  Evidence: the first draft of the `AGENTS.md` Commands entry wrote the trial root as a backticked absolute path beginning with a shell variable; that span contains a slash and none of the excluded characters, so `tools/checks/doc-integrity` would have extracted it and failed to resolve it. The line was rewritten to say "outside this tree" and to leave the default root to the driver and its card, which is where it belongs; `./tools/verify` then reported `doc-integrity: ok — 372 of 375 references resolved in 47 artifacts`, up five resolved references from 367 of 370 with no new allowlist entry. No payload or check change was made: the definition is the card's, and naming an absolute path in a map entry was the mistake.

- Observation: the layout part's violation block can say what arrived instead of the expected file, which makes the renamed-procedure case readable without opening the copy.
  Evidence: with `skills/harness-init/SKILL.md` renamed, `check <trial>/repo layout` exited 1 printing `layout: skills/harness-init/SKILL.md is missing from the copy.`, then `found skills/harness-init/README.md instead.`, then the line about the layout being fixed by every supported harness at once, and the summary `layout: the copy carries 5 of 6 procedures in the layout every supported harness requires.` That same `new` run printed `19 payload files and 5 procedures`, because the procedure count in `new` counts entry-point files while the layout part counts the directories this checkout has.

- Observation: ticking M1 confirms the diagnosis of the empty count above — the number appears as soon as one entry is complete.
  Evidence: after the Progress entry for M1 was written, `./tools/verify` printed `evidence-check: plans/active/blueprint-live-trial.md — … 1 of 7 Progress entries complete, every one timestamped.` The defect is exactly the unticked case, which is the state every plan is in on the day it lands, and it stays routed to M7.

Observed during M2 execution, 2026-09-17, in the Claude Code trial at `$HOME/blueprint-trials/claude-code-20260917-152930`. Everything above this marker predates any harness run.

- Observation: the harness refused its first non-interactive invocation for a billing reason rather than a payload reason, and it exited zero while doing it, so the refusal is invisible to anything that trusts an exit status.
  Evidence: the invocation under Concrete Steps ran for 5.7 seconds and wrote two lines to `logs/01-bootstrap.txt` — `Warning: no stdin data received in 3s, proceeding without it.` and `Fable 5.1 requires usage credits. Switch to another model, or manage usage credits at claude.ai/settings/usage?from=cc_cli_limit_message, to continue.` — with exit status 0 and no commit in the copy. `~/.claude/settings.json` on this machine names `claude-fable-5-1[1m]` as the default model. A probe run in `/tmp`, `claude -p --model opus "Reply with the single word ok." < /dev/null`, printed `ok`, and the same probe with `sonnet` printed `ok`, so the refusal is per-model and not per-account. The invocation recorded for this trial therefore adds `--model opus` and `< /dev/null`, the second because the warning names the fix. The re-run overwrote that transcript file with the session that worked, so those two lines survive here and nowhere else.

- Observation: Claude Code did not surface the six procedures through its own skills mechanism. The session found them by listing the tree and read the bootstrap procedure in its second tool call, four minutes before the file that makes them harness-visible existed.
  Evidence: the harness's own session log for the trial directory records `Bash` as the only tool the session used and, in order, `find . -path ./.git -prune -o -type f -print | sort` at 20:31:02 and `cat AGENTS.md && echo "=====SKILL harness-init=====" && cat skills/harness-init/SKILL.md` at 20:31:06. The copy arrived with no harness configuration directory; `git show --stat ae00403` lists `.claude/skills | 1 +` among the session's own changes, written at 20:35:04 as step 9 of the procedure it was already following, and `ls -l .claude/` shows `skills -> ../skills`. This machine's `~/.claude/skills` holds thirteen unrelated skills of its owner and none of the six. That is the `D4` fact for this harness: the route was the tree itself, not the harness's discovery root, and not the map — `template/AGENTS.md` is forbidden to name `skills/`.

- Observation: a correctly bootstrapped project produces dangling references by design, and the payload contradicts itself about them: the bootstrap procedure's verification step forbids them while the reference card blesses them.
  Evidence: `./tools/blueprint-eval check <trial>/repo` after the bootstrap printed `layout: ok — 6 procedures`, `fill: ok — no authoring scaffolding in 23 markdown files of the copy.` and `references: 33 dangling references in 149 examined across 21 markdown files of the copy.` Thirty-two of the thirty-three cite `tally/core/`, `tally/store.py` or `tally/cli.py` — the layering the owner brief prescribed, written as path rules a checker can evaluate — from `AGENTS.md` (5 citations), `ARCHITECTURE.md` (18), `GOALS.md` (1), `docs/DEBT.md` (6) and `docs/PRINCIPLES.md` (2). `skills/harness-init/SKILL.md` step 10 requires that "Every backticked repository-relative path across the filled artifacts resolves" and its stop condition requires those checks to have "come back clean", while `template/docs/capabilities/doc-integrity.md` names as its fourth legitimate class "a deliberate mention of a file that does not exist or must not exist", carried as an allowlist because no checker can judge it. The session reported the shortfall instead of hiding it — "The reference walk found 37 unresolved backticked paths out of 132. All 37 are accounted for … So it is not a clean run" — and seeded the copy's `docs/DEBT.md` D3 with the three pairs and the instruction to delete them when the files land. This is the first defect the trial found that reading the payload from inside this tree had not.

- Observation: the reference definition extracts an absolute system path as a reference to resolve, which M1 saw once in this repository's own prose and the trial has now seen again in a target project, where rewriting the sentence is not an option available to us.
  Evidence: the thirty-third dangling reference is `/tmp`, cited by `docs/PRINCIPLES.md:17` of the copy, written by the session while stating that the store must live inside the checkout unless the environment variable moves it. In M1 the same class of mention was resolved by rewriting this repository's own `AGENTS.md` line; the copy is scratch and its prose is the target project's. The definition's owner is `docs/capabilities/doc-integrity.md` in both halves, so excluding a span that begins with a slash is a card change with its own acceptance and is routed to M7 rather than taken here.

- Observation: the procedure's right-sizing clause was exercised by something other than its author for the first time, and the deletion it produced left nothing dangling.
  Evidence: `git show --stat ae00403` shows `docs/capabilities/prose-duplication.md | 105 ----` deleted in the same commit as the filled artifacts. The copy's `docs/capabilities/index.md` carries five rows, all `specced`, and a closing paragraph naming the deletion with the reason recorded in `GOALS.md` under scope; the references part reported no dangling reference to the deleted card, so the register row and the card file went together. The session kept `evidence-check` and `doc-integrity` beyond the three the brief required and gave a reason for each.

- Observation: the filled guide respected both the length cap and the clause a session wanting a tidy map would most easily fudge — publishing a verification command nobody ran.
  Evidence: `wc -l AGENTS.md` in the copy printed 67. Its Commands section reads "This project has no cheap verification command yet. It has no code, no test command, and no build; nothing in this repository runs.", names `D2` and the `fast-verify` card, and records one environment fact, the system `python3` version observed on this machine. The map's last entry names `skills/`, which the payload skeleton cannot carry and step 8 of the procedure adds. `docs/DEBT.md` in the copy carries `D1` for the absent code, `D2` for the missing command, `D3` for the reserved paths and `D4` for the unenforced cards.

- Observation: the Locate step that a trial makes unfollowable produced no visible friction at all — the session neither followed it nor remarked on it.
  Evidence: `skills/harness-init/SKILL.md` lines 30 to 36 tell a session that the artifacts come from the checkout it invoked the procedure from, and that that checkout's own map names the directory holding them, neither of which is true inside a trial repository. The harness's session log for this run holds 21 tool calls, 4 assistant text blocks and 14 thinking blocks stored empty; searching every stored block for "locate", "invoked from", "copy step", "re-copy" and "already satisfied" matches the operator's brief and nothing else. The session went from the `find` listing straight to reading the procedure and then to filling the artifacts already present, and its closing report does not mention the step. The record for this harness is therefore: unfollowable instruction, silently skipped, with the brief's "The copy step is already done" carrying the weight. Whether the procedure should say what a session does when the payload is already in place is still open, now with one harness's behaviour behind the question instead of none.

Observed during M3 execution, 2026-09-17, in the same Claude Code trial repository: the authoring session captured to `logs/02-author.txt` and the execution session captured to `logs/03-execute.txt`. Everything above this marker was observed before any plan existed in the copy.

- Observation: the authoring session stopped where the brief told it to and ticked a Progress entry for the act of authoring, which is not a milestone — so the copy's plan carries a ticked entry before any milestone has run, and observation 5's "exactly one entry ticked" has to be judged against the milestone entries rather than the list.
  Evidence: the session ran 8 minutes 3 seconds and exited 0. `git -C <trial>/repo log --oneline` then showed `c82847f Author the ExecPlan for tally's first working version` over the bootstrap commit; `git show --stat c82847f` listed one file, `plans/active/first-working-tally.md` at 881 insertions, and `git status --porcelain` printed nothing. The copy's Progress opens with `- [x] (2026-09-17 21:42Z) Plan authored: five milestones, contracts fixed, acceptance written for each. No implementation performed.` and then five unticked milestone entries, M1 through M5. The tick is defensible under the convention the copy carries, which requires every stopping point to be documented, and it is recorded here so that the count this milestone reports cannot be read as a session that ran two milestones.

- Observation: the evidence discipline reached the target project at authoring time, not just at execution time: the authored plan labels its transcripts expected, except for one command the session actually ran, which it ran outside the copy because the copy has no code to run anything against.
  Evidence: the copy's `Surprises & Discoveries` holds one entry, that `python3 -m unittest discover -s tests -t .` exits with `ImportError: Start directory is not importable` unless `tests/__init__.py` exists, with the failure text and the passing `Ran 1 test in 0.070s` / `OK` both quoted and the location stated as a scratch directory outside the repository. Its M1 acceptance then requires the end-to-end test to be observed failing before the package exists and passing after, and requires the project's verification command to be observed failing on a deliberately broken assertion — the two-part demonstration the copy's own card format asks for, written by a session nobody told about that requirement.

- Observation: the payload carried a feature to a running command. `tally` works from a terminal in the copy, which is the thing no amount of reading this repository could establish.
  Evidence: in `<trial>/repo`, `python3 -m tally add build && python3 -m tally add build && python3 -m tally add ship && python3 -m tally report` printed two lines, `build 2` then `ship 1`, and exited 0. The store it created is inside the checkout at `.tally/entries.log`, holding the three lines `build`, `build`, `ship`; the copy's plan fixed that path as a contract and its `.gitignore` covers `.tally/`, so `git status --porcelain` printed nothing with the file present. `TALLY_STORE=$(mktemp -d)/s.tsv python3 -m tally report` then printed nothing and exited 0, so the default is an override rather than a hard-coded location. The project's own published command, `./verify` from the copy's root, printed `verify: running tests`, seven dots, `Ran 7 tests in 0.112s`, `OK` and `verify: 1 check passed (tests).`, exited 0, and took 0.359 seconds real under `time` against the sixty-second budget its own `AGENTS.md` publishes. That guide is 71 lines and its Commands section now names `./verify`, the budget and the observed time, where at bootstrap it said the project had no such command.

- Observation: the executed milestone drove the dangling-reference count from 33 to 1, which settles the judgement M2 made about when observation 3 is judged — and the one survivor is the absolute-path class, not anything the target project got wrong.
  Evidence: `./tools/blueprint-eval check <trial>/repo references` exited 1 with a single block, `the copy cites /tmp, which the copy does not contain (cited by docs/PRINCIPLES.md:17).`, and the summary `references: 1 dangling reference in 161 examined across 21 markdown files of the copy.` The cited line states the target's own store rule — the store lives inside the checkout unless `TALLY_STORE` names somewhere else — and lists `/tmp` among the constants outside the checkout that violate it. All 32 reserved-path citations resolved once the milestone created `tally/core/`, `tally/store.py` and `tally/cli.py`, and the examined count rose from 149 to 161 as the new plan and the new files added citations of their own. The routing M2 chose holds: the definition's owner is `docs/capabilities/doc-integrity.md` in both halves and M7 carries the change. What M3 adds to that routing is the consequence nobody had noticed — that change edits `template/`, which the re-run rule makes a payload edit invalidating every harness record, so it cannot be a close-out edit made after the trials are recorded. It lands with M8's fix or it becomes its own milestone.

- Observation: the executed milestone's recorded evidence is distinguishable from the plan's own predictions by machine, which is what observation 4 asks for — but not by the presence of an expected message, only by output no prediction could have contained.
  Evidence: `git diff c82847f 0110215 -- plans/active/first-working-tally.md` reports 171 insertions and 9 deletions in that one file, the only file the execution session's last commit touched in the plan directory. Three added lines appear zero times in the authoring blob and at least once after execution: `./verify  0.11s user 0.04s system 30% cpu 0.495 total`, `FAILED (failures=3, errors=1)`, and `Ran 7 tests in 0.104s`. The line that cuts the other way is `ModuleNotFoundError: No module named 'tally'`, which the authoring version already carried once as a prediction and the executed version carries twice — so an expected error string proves nothing about who observed it, and a wall-clock figure or a failure count proves it immediately. Judging observation 4 from git, as this milestone's acceptance requires, works; judging it by searching for the messages the plan predicted would not.

- Observation: the session stopped after one milestone, and the tick count that proves it needs reading with the copy's own convention in hand: one milestone ticked, nine nested steps ticked under it, four milestones open.
  Evidence: the copy's plan carries two top-level ticked entries — the authoring entry recorded above and `- [x] (2026-09-17 21:52Z) M1 — add and report work end to end` — nine ticked sub-entries, all of them M1's granular steps with their own timestamps from 21:43Z to 21:52Z, and four unticked entries, M2 through M5. `git -C <trial>/repo log --oneline` shows four commits prefixed `M1:` over the authoring commit and nothing else. None of the files the later milestones create exists: `ls tally/core` lists `__init__.py` and `counting.py` with no `labels.py`, there is no `tools/` directory for M4's checker, and `docs/specs/` holds only `index.md`. The header paragraph the session wrote above its Progress list says M2 through M5 have not been started and that the files they create do not exist, which is checkable and checked.

- Observation: the executing session found a hole in its own plan's contract, fixed it, and wrote the deviation down rather than letting the tree carry it silently — with the state of the copy proving the fix rather than the claim.
  Evidence: the plan it was executing specified `.gitignore` as the single line `.tally/`, which does not cover the `__pycache__` directories a test run leaves behind, and nine `.pyc` files were committed before the session noticed. `git show --stat a3df528` shows that commit as one insertion to `.gitignore` and nine binary deletions, its message reading `M1: keep compiled bytecode out of the tree`; the copy's `.gitignore` now holds `.tally/` and `__pycache__/`, its Decision Log carries the departure from the plan's letter with the reason for untracking rather than rewriting history, and its `Surprises & Discoveries` carries the observation with `git ls-files | grep -c pycache` printing `9` as the evidence. Running `./verify` in the copy and then `git status --porcelain` printed nothing, so the repeated-run hygiene the fix was for holds.

Observed during M4 execution, 2026-09-17, in the omp trial at `$HOME/blueprint-trials/omp-20260917-170102`. Everything above this marker belongs to the Claude Code trial.

- Observation: omp surfaced none of the copy's six procedures at startup and surfaced twenty unrelated ones belonging to the machine's owner instead, so in this harness too the route to the bootstrap procedure was the tree.
  Evidence: a probe copy built with `./tools/blueprint-eval new omp-skills-probe` and one read-only session in it — the prompt asked it to list every skill the session was given at startup and to touch no file — reported twenty skills registered from three roots, the owner's personal skills directory under their Claude configuration, a local plugin marketplace and an Atlassian plugin cache, and said of the arrived set: "This repo has its own `skills/` directory — `doc-garden`, `harness-init`, `retro`, `capability-build`, `plan-execute`, `plan-author` — and **none of those were loaded** into this session. They are not at a discovered skill root". The bootstrap session's own transcript, stored by the harness as one JSONL file per session, records `bash` running `find . -type f -not -path './.git/*' | sort` at 22:01:26.601Z and the read of `skills/harness-init/SKILL.md` at 22:01:30.683Z, four seconds later, under the assistant line "The repository has its own `harness-init` procedure." That is six minutes before the commit that created the link meant to make them discoverable. The `D4` answer for omp is therefore the same as for Claude Code — the tree, not the harness — and it is now observed rather than inferred in both.

- Observation: step 9's first clause produced a second guide file in Claude Code and correctly produced none here, and the markdown count of the two copies is the evidence.
  Evidence: the Claude copy's root carries `CLAUDE.md` whose entire content is the line `@AGENTS.md`; the omp copy's root carries no second guide, and its session reported that this environment already loads `AGENTS.md` itself. At bootstrap the Claude copy held 23 markdown files and this one held 22, and that one file is the shim. Both sessions read the same sentence, which makes the shim conditional on the environment not reading the guide on its own, and both decided it correctly for the harness they were in.

- Observation: the two harnesses right-sized the capability register identically, without either knowing what the other did — same card dropped, same two kept beyond the three the brief names, and a debt row opened for the drop in both.
  Evidence: `git show --stat 47505cb` deletes `docs/capabilities/prose-duplication.md`, 105 lines, in the same commit as the filled artifacts, exactly as `ae00403` did in the Claude trial. The omp copy's register carries five rows, all `specced` — `fast-verify`, `evidence-check`, `doc-integrity`, `boundary-lint`, `isolated-env` — which is the same five the Claude session kept, and its `docs/DEBT.md` `D4` row reads that duplicated prose across the artifacts is caught by review only. The card's right-sizing clause has now been exercised twice by sessions that did not write it and converged.

- Observation: at bootstrap this harness's dangling references are the target's reserved code paths alone, with no absolute-path citation at all, so the class M8's fifth check exists to exclude is produced by some sessions and not by others.
  Evidence: `./tools/blueprint-eval check <trial>/repo` after the bootstrap printed `layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.`, `fill: ok — no authoring scaffolding in 22 markdown files of the copy.` and `references: 25 dangling references in 144 examined across 20 markdown files of the copy.` All 25 cite `tally/`, `tally/cli.py`, `tally/core/` or `tally/store.py` — 1 from `AGENTS.md`, 16 from `ARCHITECTURE.md` and 8 from `docs/DEBT.md` — where the Claude trial produced 33 of which the thirty-third was the `/tmp` citation in its own `docs/PRINCIPLES.md`. M8's third check therefore cannot be met by choosing a lucky harness: it has to be met by the exclusion its fifth check lands.

- Observation: the bootstrap session cost 6 minutes 29 seconds against Claude Code's 4 minutes 57, and it wrote one commit with the same shape.
  Evidence: the driven line exited 0 after 389 seconds and committed `47505cb Bootstrap the agent harness for tally`, ten files changed, 199 insertions and 588 deletions, the deletions being the skeleton guidance the fill replaced. `git -C <trial>/repo log --oneline` shows that commit over `3975c96 Receive the payload` and nothing else, and `git status --porcelain` in the copy printed nothing.

- Observation: the link the bootstrap wrote does work, and the two probes together turn observation 1 in this harness from an inference into a controlled comparison: before the link, none of the six were in the startup set; after it, all six were, loaded from the copy's own procedure directory.
  Evidence: the same read-only prompt, run in the bootstrapped copy after the trial, reported 26 skills at startup and opened with "This repository's own procedures WERE given to me at startup — all six of them, from `/Users/brennanholzer/blueprint-trials/omp-20260917-170102/repo/skills/`", listing `capability-build`, `doc-garden`, `harness-init`, `plan-author`, `plan-execute` and `retro`; the probe on the unbootstrapped copy, whose tree carries the identical procedure directory and no link, reported 20 and named none of the six. The only difference between the two trees is the `.omp/skills` link that step 9 told the session to make, so the second clause of that step is load-bearing in this harness and was satisfied by a session that had to pick the location itself. Both probes ran with the trial repository's own default configuration and wrote nothing: `git status --porcelain` in the copy printed nothing afterwards.

- Observation: whether a harness surfaced the procedures is not visible in what a print-mode run prints, and it is not visible in the stored transcript either — it took a second session to ask.
  Evidence: `<trial>/logs/01-bootstrap.txt` holds the session's closing report and nothing about its own startup set, and the stored JSONL for that session opens with the user message: there is no record of the system prompt or of what was registered into it. The first draft of this milestone's record would therefore have had to infer the answer from the session reading the tree, which proves only that the tree works. The probe pair above is what makes the claim observed, and it is cheap — 33 and 63 seconds against the 389 of the run it explains.

- Observation: the executed milestone left one dangling reference, and it is a third class — neither a reserved path that the code has since created nor an absolute system path, but a map line naming the layers in shorthand relative to a directory the reader is expected to carry in their head.
  Evidence: `./tools/blueprint-eval check <trial>/repo references` exited 1 with one block, `the copy cites core/, which the copy does not contain (cited by AGENTS.md:16).`, and the summary `references: 1 dangling reference in 160 examined across 22 markdown files of the copy.` The cited line is the copy's map entry for the tool, which names the three layers as the tool directory followed by the two module filenames and then the core directory written bare, with no package prefix on any of the three. Two of them carry no slash, so the extractor never sees them; the bare core directory does, and it cannot resolve at the repository root. All 25 of the bootstrap's reserved-path citations resolved once M1 created the package. The residue is the target project's prose, not the payload's: the project would clear it by writing the path in full or by carrying an allowlist key with a reason, which is what the reference card it kept tells it to do. The absolute-path exclusion M8 lands does not touch this class, so observation 3 fails here for a different reason than it failed in Claude Code.

- Observation: the executing session drove its own plan's acceptance harder than this plan's acceptance asks for, and it reported the milestone's remaining bluntness rather than letting the tick imply completeness.
  Evidence: the copy's Progress entry for M1 carries nine acceptance rows all run and none carved out, and then states in the same entry that a filesystem failure still exits 1 carrying the operating system's wording — quoting `tally: cannot write /nonexistent/dir/store: No such file or directory` — and that `report` does not yet refuse a store line that is not a valid label, both being M2's work. `./check` in the copy printed `Ran 36 tests in 0.334s` and `OK`, exit 0, against the 7 tests the Claude trial's first milestone landed.

- Observation: the target project's two sessions disagreed with this plan's guesses about its own names, which is the right way round: the contract belongs to the project.
  Evidence: the plan's target specification names `.tally/store.tsv` as the store path and `python3 -m unittest discover -s tests -t . -q` as the verify command. The omp trial's authoring session fixed the store at `.tally-store` and published the command as `./check`; the Claude trial fixed `.tally/entries.log` and `./verify`. In both, `python3 -m tally add build` twice, `add ship` and `report` printed `build 2` then `ship 1` and exited 0, and `TALLY_STORE` pointing at an empty path printed nothing and exited 0. Three names, one behavior: the plan's clause that nothing is pinned about the store's format was the clause that let both trials be about the payload rather than about compliance with this file.

- Observation: the step-10 contradiction M2 found was met here by a session that simply declared the reserved paths a legitimate class and moved on, which is the resolution M8 is about to write into the step — reached independently by a session that had only the pre-fix text.
  Evidence: the bootstrap report in `<trial>/logs/01-bootstrap.txt` heads its verification section "Verification run (not read)" and records "Backticked path references in `AGENTS.md`, `GOALS.md`, `ARCHITECTURE.md`, `plans/PLANS.md`, and all of `docs/`: 91 resolved, 0 unresolved in any file this pass filled. Eight unresolved remain, all inside the unedited format docs and cards, and all in the classes `doc-integrity` declares legitimate … `tally/*` paths are the deliberate class: files the map specifies and the first plan creates." It counted the reserved paths out of its own walk rather than reporting them as failures, where the Claude session reported its run as not clean and opened a debt row. Two sessions, two readings of the same step, both defensible: that is the ambiguity M8's first check removes, and neither reading is a defect in the session that made it.

Observed during M5 execution, 2026-09-17, against this machine's Codex CLI installation and the trial repository `$HOME/blueprint-trials/codex-20260917-174227`. Everything above this marker belongs to the omp trial, and nothing below it was produced by a driven session: this harness refused to run at all, and the entries record the refusal and what it took to establish it.

- Observation: the invocation this plan carried for Codex CLI does not parse. In `codex-cli 0.150.1` the sandbox flag and the approval flag are mutually exclusive, so the line labelled expected under Concrete Steps could never have run as written.
  Evidence: `codex exec -C <probe> -s workspace-write --approve-for-me "x" < /dev/null` exited 2 before any model call, printing `error: the argument '--sandbox <SANDBOX_MODE>' cannot be used with '--approve-for-me'` and the usage summary. The approval flag's own help text says it routes approval requests "through automatic review using the workspace-write sandbox", so the sandbox is implied by it rather than passed beside it, and the corrected line drops `-s`. A run that got past argument parsing confirms the implication in its banner: `sandbox: workspace-write [workdir, /tmp, $TMPDIR]`, with `approval: on-request`.

- Observation: the configured default model is refused by the service for this CLI version, and unlike the Claude Code refusal M2 recorded, this one exits nonzero — so the same class of failure is invisible to an exit status in one harness and visible in the other.
  Evidence: `~/.codex/config.toml` sets `model = "gpt-6-astra"` with `model_reasoning_effort = "high"`. `codex exec -C <probe> --approve-for-me "Reply with the single word ok." < /dev/null` exited 1 after 5 seconds, printing `warning: Model metadata for` that model `not found. Defaulting to fallback metadata` and then, twice, `ERROR: {"type":"error","status":400,"error":{"type":"invalid_request_error","message":"The 'gpt-6-astra' model requires a newer version of Codex. Please upgrade to the latest app or CLI and try again."}}`.

- Observation: every model this account can reach is out of quota until three days after this session, on the non-interactive path and on the interactive one alike, so Codex CLI cannot be driven at all today. This is the case the Concrete Steps paragraph anticipated, reached for the first time.
  Evidence: six models named in `~/.codex/models_cache.json` were probed one at a time with the prompt `Reply with the single word ok.` — `gpt-5.6-sol`, `gpt-5.5`, `gpt-5.6-luna`, `gpt-5.6-terra`, `gpt-daybreak-blue-latest` and `gpt-reserve` — and each exited 1 printing `ERROR: You've hit your usage limit. Visit https://chatgpt.com/codex/settings/usage to purchase more credits or try again at Sep 20th, 2026 7:12 PM.` A seventh, `gpt-5.2-codex`, exited 1 with `The 'gpt-5.2-codex' model is not supported when using Codex with a ChatGPT account.` There is no API-key path to fall back to: `~/.codex/auth.json` reports `auth_mode` as `chatgpt` with its `OPENAI_API_KEY` field null. The interactive path reaches the same wall: `codex --cd <probe> "Reply with the single word ok."` driven under a pseudo-terminal asked to trust the directory, then to review six new or changed hooks, then loaded the configured model and printed the same version error together with an offer to switch to a cheaper model for rate-limit reasons. The operator confirmed independently that the account's usage is exhausted; that part is reported rather than observed here.

- Observation: Codex CLI does have a skills mechanism in this version, and it is on by default. The premise M5 was written against — a harness whose help text names no skills mechanism at all, which makes the map the only route to a procedure — survives only as a statement about the help text.
  Evidence: `codex exec --help | grep -ci skill` still prints `0`, as it did when this plan was authored, but `codex features list` reports `skill_search` as `stable` and `true`, alongside `skill_mcp_dependency_install`; `~/.codex/skills/.system` holds six system skills — `imagegen`, `openai-docs`, `plugin-creator`, `review-agent`, `skill-creator`, `skill-installer` — with no user skill beside them, where this machine's Claude configuration holds thirteen of its owner's and its omp configuration holds none; and the installed binary carries a skill-root alias mechanism whose default root map includes the workspace-relative `skills/` directory, with instructions to expand a listed short path against a named root and read that skill's entry-point file to the end before acting. Whether that root is resolved against the session's working directory, and whether it would therefore surface the copy's six procedures with no edit to any file, is exactly the observation-1 question M5 owes and exactly what no static reading can settle — in omp it took two live probe sessions to separate the harness's own roots from the link the bootstrap wrote.

- Observation: `codex exec` announces that it is reading standard input even when standard input is closed, so the warning Claude Code emitted for a real three-second wait has a look-alike here that means nothing.
  Evidence: the run above was invoked with `< /dev/null` and still printed `Reading additional input from stdin...` as its first line, once, before the banner; the prompt it used was the command-line argument. The recorded invocation keeps `< /dev/null` for the reason the other two harnesses keep it — a driver has no input to give — and not because this message goes away.

- Observation: the trial repository built for this harness reproduces M1's measurement of a fresh copy exactly, so what waits for the driven sessions is a valid unfilled payload and not a botched build.
  Evidence: `./tools/blueprint-eval new codex` printed `copied 25 files — 19 payload files and 6 procedures — committed as "Receive the payload".` and `./tools/blueprint-eval check <trial>/repo` over it printed `layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.`, `fill: 117 marker lines in 8 of 23 markdown files.` and `references: ok — 144 of 146 references resolved inside the copy, 1 allowlist entry applied, 0 stale.` — the same three numbers M1 observed on a smoke copy. The three briefs were extracted from this file's own indented blocks into `<trial>/logs/` at 62, 21 and 6 lines with no indentation left on any line, the counts M4's extraction produced, and `grep -rc 'harness-blueprint' <trial>/logs/` printed `0` for each of the three. `git -C <trial>/repo log --oneline` shows one commit, `60162df Receive the payload`, and `git status --porcelain` in the copy printed nothing.

Observed during M8 execution, 2026-09-21, in this repository and in the two post-fix trial repositories built for it. Everything above this marker belongs to the Codex CLI entries M5 left behind.

- Observation: the absolute-path exclusion changes the driver's answer on identical input, and it changes the denominator rather than the allowlist — the citation stops being extracted at all instead of being extracted and excused.
  Evidence: a fresh copy at `$HOME/blueprint-trials/abs-exclusion-20260921-152822` had one line appended to its own `docs/PRINCIPLES.md` reading that scratch files go under a backticked `/tmp/tally-scratch`, which is the shape M3 observed in the Claude Code trial. The pre-fix driver, run from `git show 48dc067:tools/blueprint-eval`, exited 1 with `the copy cites /tmp/tally-scratch, which the copy does not contain (cited by docs/PRINCIPLES.md:71).` and the summary `1 dangling reference in 147 examined across 21 markdown files of the copy.` The committed driver over the same copy printed `ok — 144 of 146 references resolved inside the copy, 1 allowlist entry applied, 0 stale.` — 146 examined against the pre-fix 147, and `tools/allow/blueprint-eval.txt` untouched in both runs.

- Observation: the fix costs this repository's own reference count nothing, which is the evidence that no live artifact here was relying on an absolute path resolving by accident.
  Evidence: `./tools/verify` before the two commits and after them both printed `doc-integrity: ok — 372 of 375 references resolved in 47 artifacts, 2 format documents skipped, 3 allowlist entries applied, 0 stale.` The live check resolved an absolute path silently before the fix — `test -e /tmp` is true on this machine from any directory — so the class was a false pass here and a false failure in the trial copies, and the same one-line exclusion settles both.

- Observation: the fixed step produced the artifact it asks for on the first session that read it. The harness that had reported its bootstrap as not clean now reports the class by name, seeds the allowlist keys, and states the event that retires them.
  Evidence: the post-fix Claude Code bootstrap, harness `2.1.274 (Claude Code)`, ran 4 minutes 55 seconds in `$HOME/blueprint-trials/claude-code-postfix-20260921-152808`, exited 0 and committed `2b2691b Bootstrap the agent harness for tally` over `da09bea Receive the payload`, with `git status --porcelain` in the copy printing nothing. Its report reads "D2 — the reference walk did not come back clean, and was not expected to. Over the 14 filled artifacts: 104 references, 23 unresolved. Twenty are `tally/core/`, `tally/store.py`, `tally/cli.py` — reserved by the layering the owners stated, with the exact allowlist keys written into D2 so a check built before the code arrives has them." The copy's `docs/DEBT.md` carries the row `D2 | Artifacts cite module paths that do not exist yet` whose trigger column reads that the first plan creates the three paths, and its Details section lists seven allowlist keys with the reason to write beside each, plus an eighth key of the permanent class that never leaves.

- Observation: the debt row the fixed step asks for cites the reserved paths itself, so writing it makes the same walk report more of them — seven of this bootstrap's twenty citations are the row that explains the other thirteen.
  Evidence: `./tools/blueprint-eval check <trial>/repo` after the bootstrap printed `layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.`, `fill: ok — no authoring scaffolding in 23 markdown files of the copy.` and `references: 20 dangling references in 136 examined across 21 markdown files of the copy.` The citing lines are `ARCHITECTURE.md` twelve times, `docs/DEBT.md` seven — lines 8, 36 and 37, which are the register row and its Details paragraph — and `docs/PRINCIPLES.md` once. Every one of the twenty cites `tally/core/`, `tally/store.py` or `tally/cli.py`; no absolute path is cited anywhere in the copy, where the pre-fix trial of the same harness ended with one. The count is a property of the payload's own remedy and not a defect: a row that names the paths is worth more than a walk that is one citation shorter.

- Observation: the two harnesses converged again, this time on the remedy the fixed step names — a debt row, the exact allowlist keys, and the event that deletes them — and neither of them reported its bootstrap as unclean the way the pre-fix Claude Code session did.
  Evidence: the omp re-run, harness `omp/18.1.14`, ran 5 minutes 26 seconds in `$HOME/blueprint-trials/omp-postfix-20260921-152834`, exited 0 and committed `e88f00c` over `b5ee65d Receive the payload`. Its report reads "Reference walk over 16 artifacts: 114 references, 29 unresolved. 23 are the reserved `tally/…` citations (8 distinct file-and-path pairs)", and its `docs/DEBT.md` `D2` row reads that the architecture names module paths that do not exist, listing eight keys from `AGENTS.md:tally/` to `docs/DEBT.md:tally/core/` and closing with the sentence that deleting an entry while the path is still missing and rewording an artifact to stop naming it both fail. The Claude Code re-run wrote seven keys plus one permanent key of the card's own class. Two sessions, two harnesses, one shape, and neither had the other's output.

- Observation: the driver's numbers separate what the fix changed from what the session chose. Both re-runs report reserved paths alone, and the two counts differ only because the two sessions cite them in different places.
  Evidence: Claude Code 20 dangling in 136 examined across 21 files; omp 23 dangling in 136 examined across 20 files. The examined counts are identical, the citing sets are not: omp's artifacts cite the bare package directory `tally/` three times where Claude's never do, and omp's `AGENTS.md` carries one of them. Neither copy cites an absolute path, and neither carries the bare `core/` shorthand the pre-fix omp trial ended on — that citation was one session's map line, which is why the M4 decision left it with the target project rather than allowlisting it here.

- Observation: the same harness resolved step 9's second clause differently on two runs, and the trial has no opinion about which was right.
  Evidence: the pre-fix omp session made `.omp/skills`, and the post-fix one made `.claude/skills` pointing at `../skills` — `ls -a <trial>/repo` in the new copy lists `.claude` and no `.omp`, and the session's report says this environment loads procedures from its own location and names all six as reachable through the link. Step 9 tells a session to make the installed set reachable where the environment it is running in looks, and deliberately does not name a location, so both choices satisfy its text. The discovery record this plan owes is M4's probe pair, which was run against the pre-fix copy and is not re-run here: the fix does not touch step 9.

Observed during M5's resumed session, 2026-09-21, in the Codex CLI trial at `$HOME/blueprint-trials/codex-20260921-154505` and in the probe copy beside it. Everything above this marker belongs to M8's post-fix bootstraps; everything below was produced by the three driven sessions M5's first session could not run.

- Observation: the account-level refusal expired and the version-level one did not, so the two walls M5's first session hit are separable and only one of them has moved.
  Evidence: `codex exec -C <probe> -m gpt-5.6-sol --approve-for-me "Reply with the single word ok." < /dev/null` exited 0 printing `ok`, and so did the same line with `gpt-5.6-luna` and with `gpt-5.5`, three probes in 15 seconds altogether. The same line with no model flag exited 1 in 3.9 seconds, warning that metadata for `gpt-6-astra` was not found and printing twice that this model `requires a newer version of Codex`. Four days on, every model the account can reach answers; the configured default still refuses.

- Observation: `codex exec` refuses to start outside a git repository, before any model call and before any prompt is read.
  Evidence: the first probe was run in a plain `/tmp` directory and exited 1 in 0.17 seconds with `Not inside a trusted directory and --skip-git-repo-check was not specified.` Re-running it in the same directory after `git init -q` reached the model. This never touched the trial, because the driver commits the copy before any session sees it, but it is the third refusal this harness produces that has nothing to do with the payload, and the only one that costs no tokens.

- Observation: the corrected invocation drove all three sessions end to end, and this harness is the slowest of the three at the same work by a factor of nearly two against the fastest.
  Evidence: `codex exec -C <trial>/repo -m gpt-5.6-sol --approve-for-me "$(cat ../logs/brief-…)" < /dev/null`, three separate processes with standard input closed and no resumption, each exiting 0: bootstrap 650 seconds committing `60ebbe6`, authoring 595 seconds committing `74d18e5` with 232 insertions in one plan file and nothing else, execution 561 seconds committing `bb405f3`. Thirty minutes six seconds of driven work against omp's 29 minutes and Claude Code's 18.

- Observation: Codex surfaced none of the copy's six procedures at startup, before or after the bootstrap, and this is the first trial in which the bootstrap session installed nothing anywhere — no link, no configuration, no second guide — and the copy still carried a feature to a running command.
  Evidence: the probe pair. In a fresh copy built with `./tools/blueprint-eval new codex-skills-probe`, one read-only session reported 24 skills, five from `~/.codex/skills/.system` and nineteen from plugin caches under `~/.codex/plugins/cache/`, and named none of the six. In the bootstrapped copy after the trial, the same prompt reported 14 from the same two kinds of root — the curated-remote plugin entries absent on that run — and again named none of the six. `ls -a <trial>/repo` lists `.git`, `.gitignore` and the project's own directories: no `.codex`, no `.claude`, no `.omp`, where the Claude Code copy carries `.claude/skills` and both omp copies carry a link of their own. `git status --porcelain` printed nothing after each probe.

- Observation: the route to the procedure was the tree here too, and this is the first session in the trial to say out loud that the Locate step was already satisfied rather than passing over it in silence.
  Evidence: the session's second command was `git ls-files` — its listing ends `skills/capability-build/SKILL.md` through `skills/retro/SKILL.md` — and it then wrote: "The repository is clean and contains only the committed harness payload. I found a dedicated `harness-init` procedure, so I'm using that as the governing bootstrap workflow; its copy phase is already satisfied exactly as you specified." Its next command read `skills/harness-init/SKILL.md`. In Claude Code the same step produced no visible friction and no mention at all, which M2 recorded; here it produced one clause of acknowledgement and no friction either.

- Observation: the references part exits 0 for the first time in the whole trial, and what cleared the last citations was the target project retiring its own debt row rather than anything this repository fixed.
  Evidence: after the bootstrap, `./tools/blueprint-eval check <trial>/repo` printed `references: 6 dangling references in 114 examined across 20 markdown files of the copy.`, every one of them `tally/core/`, `tally/store.py` or `tally/cli.py`. After the executed milestone the same command printed `layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.`, `fill: ok — no authoring scaffolding in 23 markdown files of the copy.` and `references: ok — 108 of 110 references resolved inside the copy, 1 allowlist entry applied, 0 stale.`, exiting 0. The examined count falls from 114 to 110 while the package arrives, because the execution session paid both bootstrap debt rows and deleted them, and the `docs/DEBT.md` register that had cited the reserved paths seven times over is now an empty table.

- Observation: the third harness right-sized the capability register exactly as the first two did, without any of the three knowing what the others chose.
  Evidence: the bootstrap kept `fast-verify`, `evidence-check`, `doc-integrity`, `boundary-lint` and `isolated-env`, all `specced`, and dropped `prose-duplication` with the reason that its complexity is not justified for a documentation set this small. That is the same card dropped and the same five kept in all three harnesses, and the same two kept beyond the three the brief names.

- Observation: the fixed step 10 produced its artifact for the third time, and the target then retired it inside one milestone — so the remedy the step prescribes has now been seen through its whole life, from the row being written to the row being deleted because the paths exist.
  Evidence: at bootstrap the copy's `docs/DEBT.md` carried `D2 | Intended component paths do not exist`, whose Details paragraph names five allowlist keys — `ARCHITECTURE.md:tally/core/`, `ARCHITECTURE.md:tally/store.py`, `ARCHITECTURE.md:tally/cli.py`, `AGENTS.md:tally/core/`, `docs/PRINCIPLES.md:tally/core/` — and states that fixed means those paths exist and the entries are removed. After the executed milestone the register is empty and the execution session's report reads that it retired bootstrap debt `D1` and `D2`.

- Observation: a third trial produced a third set of names for the same behavior, and the first tab-separated output of the three.
  Evidence: `python3 -m tally add build` twice, `add ship` and `report` printed `build`, a tab and `2`, then `ship`, a tab and `1`, exit 0. The store is `.tally.json` inside the checkout, covered by the copy's own `.gitignore` so `git status --porcelain` printed nothing with it present, and `TALLY_STORE=$(mktemp -d)/s.json python3 -m tally report` printed nothing and exited 0. The published command is `python3 scripts/verify.py`, which printed `Ran 1 test in 0.096s`, `OK` and `fast-verify: PASS (unittest)` in 0.359 seconds real. Three trials: `.tally/entries.log` with `./verify`, `.tally-store` with `./check`, `.tally.json` with `python3 scripts/verify.py`.

- Observation: this authoring session ticked nothing at all, which removes the ambiguity M3 had to rule on — and the list still does not read one tick per milestone, for a different reason.
  Evidence: at the authoring commit `74d18e5` the copy's `Progress` holds three unticked milestone entries and no entry for the act of authoring, where both other harnesses recorded their own stop as a ticked line. After execution it holds exactly two ticked entries, both belonging to Milestone 1 — one for the red check at 21:09Z and one for the milestone at 21:11Z — and two open milestones. The decision M3 logged still does the work: the facts that decide observation 5 are the milestone entries and the commits, not the tick count.

- Observation: the machine test for observation 4 came out the same way as in the other two harnesses, including the line that cuts the other way.
  Evidence: counting fixed strings in the plan file at the authoring commit and at the execution commit, `real 0.37` goes 0 to 1, `Ran 1 test in 0.099s` goes 0 to 1, and `AssertionError: 1 != 0` goes 0 to 1, while `fast-verify: FAIL (unittest)` goes 2 to 3 — an expected failure message the authoring session had already written down twice as a prediction. A wall-clock figure proves the session observed something; a quoted error string does not.

Observed during M6 execution, 2026-09-21, in `missing-skill-claude-20260921-163125`. Everything above this line predates the harness failing case.

- Observation: the harness failing case did not fail. With the entry-point filename wrong, the first harness bootstrapped the copy anyway, and nothing in its route went through a skill loader at all.
  Evidence: the driven session exited 0 in 457 seconds and committed `5e4401c` over `d161b01`. Its stored transcript's first tool call is `find . -path ./.git -prune -o -type f -print | sort` and its second reads two files together, `AGENTS.md` and the renamed procedure, with `for f in AGENTS.md skills/harness-init/README.md; do echo "=============== $f"; cat "$f"; done`. That is the same route every harness in this trial took to a correctly named file, so the rename removed nothing the session was using. It ran longer than M8's re-run of the identical brief against the correctly named payload — 457 seconds against 295 — which one run cannot attribute to the rename.

- Observation: the target project neither renamed the arriving file nor forked it. It wrote the defect into its own debt register and named the upstream repository as the fix, which is the first time a trial has sent a finding back up the chain rather than absorbing it.
  Evidence: the copy's `docs/DEBT.md` carries `D3 | The bootstrap procedure is not loadable as a skill in this environment | skills/harness-init/README.md | It arrived under that name, and arriving procedures are not edited here — a patch applied locally becomes a fork that cannot take upstream fixes | The upstream harness renames it, or this project needs to bootstrap a second repository from this copy`. The session's own report says the same thing in one clause: "The arriving files were not edited."

- Observation: the session installed the harness link and then recorded that the link cannot surface the thing it was installed for, both in the same report — which is `D4` evidence of a kind no correctly named trial could produce.
  Evidence: the report's environment paragraph reads "`.claude/skills` → `../skills` so the installed procedures load without editing them", and eleven lines later "`skills/harness-init/README.md` is named `README.md`, not `SKILL.md`, so this environment's skill loader won't surface it — left as-is and recorded as debt rather than patched into a local fork." The link is real and the loader is real; the procedure that did the work reached the session through neither.

- Observation: on the two parts that do not read a procedure's filename, the crippled trial's copy is indistinguishable from a healthy one.
  Evidence: `./tools/blueprint-eval check <trial>/repo` printed `fill: ok — no authoring scaffolding in 23 markdown files of the copy.` and `22 dangling references in 138 examined across 21 markdown files of the copy.`, every one of the 22 a reserved `tally/` path, against the 20 of M8's re-run in this harness. Only the layout part reports anything wrong, which is the measurement behind this milestone's status decision: in a harness that reads the tree, the driver is the whole of the layout invariant.

- Observation: the register right-sizing came out the same way a fourth time, under a fourth set of circumstances.
  Evidence: the copy keeps `fast-verify`, `isolated-env`, `boundary-lint`, `evidence-check` and `doc-integrity`, and drops `prose-duplication` for the reason the other three gave — a windowing duplication checker is not worth its cost on an artifact set this small. Four bootstraps, four identical judgements, one of them made while the procedure describing the judgement sat under the wrong filename.

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

- Decision: the fill part prints one locator line per marker line, and the next-action paragraph once per file rather than once per marker.
  Rationale: a fresh copy carries 117 marker lines, and a block per marker is 234 lines of identical advice for a state the bootstrap is about to clear. Every violation still names the part, the file and the line, which is what the interface requires; the action is the same for every marker inside one artifact, so it is stated where the reader can act on it — once per artifact — and the summary line carries the counts. Observed output for the whole part on an unfilled copy: 126 lines.
  Date/Author: 2026-09-17, M1 execution session.

- Decision: `new` refuses a label that is not a single plain directory name, which is a fourth refusal beyond the three the interface lists.
  Rationale: the label is concatenated into a path under the trial root, so a label containing a slash or starting with a dot or a hyphen writes somewhere other than the directory the command promises, and the destination-exists and inside-the-worktree refusals would both be decided about the wrong path. The guard is one case statement and it fails before anything is created.
  Date/Author: 2026-09-17, M1 execution session.

- Decision: `AGENTS.md`'s Commands preamble now says two commands exist and describes what separates them, and the driver's entry does not name the default trial root.
  Rationale: the preamble said a machine's decisions here are made by one command, which the driver made false, and a map entry that contradicts the list under it routes a reader wrong. The root is left to the driver and its card for two reasons: the reference walk over `AGENTS.md` extracts a backticked path rooted in a shell variable and cannot resolve it, as the Surprises entry records, and the default belongs to the command that owns it rather than to the map.
  Date/Author: 2026-09-17, M1 execution session.

- Decision: the references observation is judged at the end of a harness's trial, after the executed milestone has landed the code the artifacts describe, and not at bootstrap; the bootstrap milestone records the composition of what dangles and the execution milestone re-runs the part.
  Rationale: `docs/capabilities/blueprint-eval.md` states its passing case as one sequence — copy, bootstrap, author a plan, execute its first milestone — and then asks for all five observations of that sequence, and only the fill observation carries a stage qualifier. At bootstrap the target has no code, and the layering the owner brief supplies is exactly what the payload asks to be written as path rules a checker can evaluate, so every trial in every harness would fail the observation for a state the payload prescribes. The alternative was to seed `tools/allow/blueprint-eval.txt` with the dangling keys, and it loses: those keys are one session's prose — `GOALS.md:tally/core/`, `docs/PRINCIPLES.md:/tmp` — the next harness cites a different set from different artifacts, and this repository's tracked allowlist would accumulate one target project's paragraph structure while reporting stale entries on every other run. M2's acceptance clause 2, which asked for all three parts to pass on the bootstrapped copy, is the clause this diverges from; the divergence is recorded in its `Progress` entry and in the clause itself.
  Date/Author: 2026-09-17, M2 execution session.

- Decision: the Claude Code sessions are driven with `--model opus` and with standard input closed, and both are recorded with the invocation rather than counted against observation 1.
  Rationale: this machine's configured default model refused the first run for billing reasons, exiting zero with nothing done, so the trial either changes model or does not run in this harness at all. The card already rules that a flag granting the harness permission is recorded with the invocation and not counted against the discovery observation; a model selection is the same class of fact — it changes who does the work, not what the copy contains or what the session can see. Opus rather than sonnet, though both answered the probe, because it is the nearest in capability to the configured default and the payload deserves judging against a frontier model rather than against the cheapest one that would start.
  Date/Author: 2026-09-17, M2 execution session.

- Decision: the payload defect this milestone found — the bootstrap procedure demanding that every backticked path resolve, against its own reference card's fourth legitimate class — is recorded here and fixed in M8, not in this session.
  Rationale: `skills/plan-execute/SKILL.md` forbids widening a milestone past its boundary, and the re-run rule above makes a mid-trial payload edit invalidate the very bootstrap record this milestone just wrote: the copy in `claude-code-20260917-152930` holds the pre-fix procedure text, so landing the fix now costs a fresh trial repository and a re-driven bootstrap before M3 can start. M3's subject is `plan-author` and `plan-execute`, which the fix does not touch, so the honest arrangement is a bootstrap record labelled as pre-fix, M3 continuing in the same copy, and one later milestone landing the fix together with the re-runs it invalidates.
  Date/Author: 2026-09-17, M2 execution session.

- Decision: observation 5 is judged against the plan's milestone entries and the commit log, not against every ticked checkbox in `Progress`.
  Rationale: the convention the payload ships requires every stopping point to be documented in `Progress` and makes checklists mandatory there, so a compliant target plan carries more ticks than milestones — the copy's plan has two top-level ticks and nine nested ones after one milestone, because the authoring session recorded its own stop and the executing session recorded nine granular steps. The clause exists to catch a session that ran two milestones, and the facts that decide that are which milestone entries are ticked and what the commits contain. Reading the clause literally would fail a target project for following the convention the payload gave it, which would make the observation a test of the payload's own inconsistency rather than of the session's discipline.
  Date/Author: 2026-09-17, M3 execution session.

- Decision: the absolute-path exclusion moves out of M7's routing paragraph and becomes M8's fifth acceptance check.
  Rationale: M2 routed the finding to close-out when the only instance was one line of scratch prose. M3 observed it as the sole surviving dangling reference of a completed trial, which makes it the difference between observation 3 passing and failing in every harness, and it is a change under `template/` — the re-run rule turns that into a re-driven bootstrap. A close-out milestone cannot invalidate the three harness records above it and then close the plan over them. M8 already exists to land a payload fix and re-run what it invalidates, and its own third check asks for a trial whose dangling references are exactly the target's reserved paths, which is unreachable while the definition still extracts `/tmp`. Both fixes therefore land in one milestone and one re-run, and M7 keeps only the verification.
  Date/Author: 2026-09-17, M3 execution session.

- Decision: the discovery observation in omp is decided by two read-only probe sessions — one in a fresh copy, one in the bootstrapped copy — rather than by reading the trial transcript, and the probes are recorded as part of the trial rather than hidden as method.
  Rationale: this milestone owes the record whether the harness surfaced the procedures automatically and from which root, and nothing in a print-mode run answers that: the captured output is the session's closing report, and the stored transcript begins at the user message with no record of what was registered into the system prompt. Inferring "not surfaced" from the session having listed the tree is the weaker claim — a session may read a file it was already given — so the question is put to the harness directly, with a prompt that names no procedure, asks for the startup set, and forbids touching a file. Two probes rather than one because a single answer cannot separate the harness's own roots from the link the bootstrap wrote; the pair is a controlled comparison whose only variable is that link. A probe is not a trial session: it authors nothing, its prompt is recorded here, and the trial's own three sessions remain the three the card asks for.
  Date/Author: 2026-09-17, M4 execution session.

- Decision: the one dangling reference the omp trial ends with is the target project's to clear, not this repository's, and observation 3 is recorded as failed in this harness on that basis rather than allowlisted away.
  Rationale: the citation is a map line in the copy that names the tool's layers in shorthand, and the bare core directory in it resolves nowhere because the project wrote a path relative to a package prefix it left off. The reference card the copy kept states the classes a project may carry deliberately and tells it to write an allowlist key with a reason; the copy did neither, which is a finding about a target project's first bootstrap and not a defect in the definition. Seeding this repository's own allowlist with the key would be the trial patching the copy, which the card forbids in the one sentence it repeats twice. The class is also not the one M8 fixes: an absolute-path exclusion leaves a bare relative directory exactly as dangling as it is today, so recording it as met-after-M8 would be a prediction, and a wrong one.
  Date/Author: 2026-09-17, M4 execution session.

- Decision: this milestone lands no payload fix and appends no re-run milestone; the two findings it produced are routed rather than fixed here.
  Rationale: M4's own text says a payload defect found in this session is fixed in this repository and costs Claude Code a re-run, so the judgement of whether there is one is owed explicitly. There is not. The bare relative directory belongs to the target project by the entry above. The step-10 ambiguity is the defect M2 already found and M8 already owns, and this session's evidence sharpens M8's first check rather than adding a second fix: the two harnesses read that step two ways, which is what makes naming the class the right repair. The absolute-path class did not recur, so M8's fifth check stands on the Claude Code evidence alone. Landing anything under `template/` or `skills/` here would invalidate the record this session just wrote as well as the one above it, for a change no finding of this session requires.
  Date/Author: 2026-09-17, M4 execution session.

- Decision: M5 is split in place rather than recorded as a harness that cannot be driven. The trial repository and the three briefs are built and left where the next session finds them, the refusal is recorded with its evidence, and the three driven sessions wait for a session run after this account's usage limit resets on 2026-09-20.
  Rationale: Concrete Steps offers exactly three responses to a harness that will not run, and two of them are unavailable here. Driving the sessions by hand fails for the same reason the driven ones do: the refusal is an account-level quota, and the interactive path reaches it after the same two trust prompts, so there is no hand to drive it with. Recording Codex CLI undrivable would state a property of the harness on the evidence of one account on one day, and it would hand M6 a status decision resting on a fact with a published expiry three days out. Two further candidates were refused outright: the local-provider flag, which would judge the payload against whatever model happened to be downloaded rather than against a frontier one and needs a server that is not running on this machine; and substituting a fourth harness, which Concrete Steps forbids in as many words. The trial repository is not stale work — it holds the payload commit and nothing else — so the resuming session starts from a clean copy rather than hand-finishing a half-bootstrapped one, which is what the Idempotence section asks for. What the split costs is that M6 and M7 both wait on M5, and that cost is smaller than a wrong record.
  Date/Author: 2026-09-17, M5 execution session.

- Decision: the premise in M5's own heading — a harness with no skills mechanism, where the map is the only route to a procedure — is corrected in place, and the resumed milestone owes the omp-style probe pair rather than that assumption.
  Rationale: the premise was measured at authoring time from the help text, and the help text still says nothing about skills; the mechanism is nonetheless present and enabled in `codex-cli 0.150.1`, with a system skills root on this machine and a default root map that includes a workspace-relative procedure directory. Leaving the sentence standing would hand the resuming session a false reason to skip the question, and observation 1 in this harness is the one the card's criterion was written for — the whole point of recording it is that nobody knows the answer. Correcting the framing is inside this milestone's boundary because it is this milestone's own text; deciding the answer is not, because only a driven session can.
  Date/Author: 2026-09-17, M5 execution session.

- Decision: (post-M5 review, routed to M7's retro rather than acted on) The
  unparseable Codex invocation is a lesson about authoring practice, not only
  a corrected line: the authoring session measured every flag from `--help`
  and never parsed the composed command, and the pre-execution review missed
  it the same way. `--help` is not a parse. The candidate rule, for the retro
  to weigh and route — likely to `skills/plan-author/SKILL.md`, whose step 8
  already demands rereading as the executor: an expected invocation in a plan
  is dry-run to the first refusal the environment can produce without doing
  the work — argument parsing, authentication, a version banner — and the
  refusal point reached is labelled beside the line. This plan's own history
  argues for it twice: the discovery criterion and the failing-case premise
  were both defects of the same shape, statements about composed behavior
  checked only component-wise.
  Date/Author: 2026-09-17, reviewer.

- Decision: the fixed step 10 names `docs/DEBT.md` as where the reserved paths are recorded and does not name the reference card, even though `tools/checks/boundary-lint` would have allowed the citation.
  Rationale: the rule the check implements resolves a procedure's reference inside the payload, and the card does resolve there, so naming it would have passed. The layer map's prose is narrower than its check: a card is project content, the payload ships a starter register, and the bootstrap procedure itself tells a session to drop a card whose property cannot exist in this project. Both recorded trials dropped one. A step that named the card would ship a dangling reference to the first project that dropped the one it named, and the check would not catch it because it decides the question against this checkout's payload rather than against the project's own register. The debt register is the one artifact every bootstrapped project keeps, so the class is described in the procedure's own words and routed to the file that is always there.
  Date/Author: 2026-09-21, M8 execution session.

- Decision: the absolute-path exclusion is applied where references are extracted, in both scripts, rather than where they are resolved.
  Rationale: the two placements differ in what the passing summary means. Excluding at extraction removes the span from the count entirely, so `144 of 146` states how many real references were examined; excusing it at resolution would either grow an allowlist with keys that are not judgements — the thing the card's fourth class exists to keep small — or add a silent resolution rule that makes the denominator include spans nobody intended as references. The observed pre-fix and post-fix runs over one identical copy differ in exactly that way: 147 examined with one dangling against 146 examined with none, and the allowlist untouched.
  Date/Author: 2026-09-21, M8 execution session.

- Decision: both invalidated bootstrap records are re-run rather than labelled, which is the first branch of this milestone's fourth acceptance check and not the cheaper second one.
  Rationale: the fix changes what a bootstrapping session is told to do about the one class both recorded sessions actually hit, so a label would leave this plan claiming a repaired procedure whose only evidence is the repair's own text. Two driven bootstraps cost 10 minutes 21 seconds between them and produce the evidence the third check asks for at the same time, since the Claude Code re-run is that check's trial. The authoring and execution halves are deliberately not re-driven: the fix touches neither `skills/plan-author/SKILL.md` nor `skills/plan-execute/SKILL.md`, and the re-run rule invalidates a record made against changed text, not every record in the plan.
  Date/Author: 2026-09-21, M8 execution session.

- Decision: the Codex CLI sessions run on `gpt-5.6-sol`, and the model flag is recorded with the invocation rather than counted against observation 1, exactly as M2 ruled for the other harness.
  Rationale: the configured default is still refused by the service for this CLI version, so this harness either names a model or does not run at all. Of the six the account can reach, two are described as fast and affordable, one is the previous generation, one is a frontier model scoped to defensive security work and one is an automatic review model, which leaves the current generation's general agentic model as the nearest thing to the refused default. Judging the payload against the cheapest model that would start would make a weak result unattributable: a session that missed the procedures would leave nobody able to say whether the payload or the model was at fault.
  Date/Author: 2026-09-21, M5 resumed session.

- Decision: the trial repository M5's first session left behind is abandoned rather than reused, and a fresh one is built from the fixed payload.
  Rationale: the plan's own Progress narrative already directed this, and the Idempotence section states the rule it follows — a trial that cannot be completed is abandoned, not repaired, because the next one starts from a clean copy for a few tenths of a second of driver time. Reusing the stale copy would have produced a Codex record made against the pre-fix procedure text, which is precisely the state M8 spent two driven bootstraps clearing in the other two harnesses. The abandoned directory is left on disk under its own timestamp so that the refusal evidence M5 recorded remains inspectable.
  Date/Author: 2026-09-21, M5 resumed session.

- Decision: observation 1 is recorded as holding in Codex CLI, and the fact that step 9's second clause produced no artifact at all here is recorded as evidence for `D4` rather than as a payload defect to fix in this milestone.
  Rationale: this is the cleanest instance of the criterion in the whole trial. The copy contains no harness configuration of any kind, so nothing in it could have helped the session find the procedures, and it named and followed the right one on its second command. The clause tells a session to make the installed set reachable where the environment it runs in looks, and in this harness the roots that are read are the user's own directories outside the repository — a session that wrote into one would be editing the machine rather than the project, which is a question about where procedures should live and not about whether this step is correct. `docs/DEBT.md` `D4` is the standing owner of that question and M7 rewrites it with what the three harnesses did; deciding it here would be this plan choosing the answer it exists to gather evidence for.
  Date/Author: 2026-09-21, M5 resumed session.

- Decision: this milestone lands no payload fix and appends no re-run milestone, and the two findings it produced are routed rather than repaired.
  Rationale: the same judgement M4 owed and answered the same way. The step 9 silence belongs to `D4` by the entry above. The harness's three non-payload refusals — a version-locked default model, a git-repository requirement, and the expired usage limit — are facts about this machine and this account, recorded under `Surprises & Discoveries` and in the invocation, and nothing in `template/` or `skills/` could repair them. Landing an edit under either directory here would invalidate the record this session just wrote and cost the two completed harnesses another pair of driven bootstraps, for no defect.
  Date/Author: 2026-09-21, M5 resumed session.

- Decision: the harness failing case is run in Claude Code.
  Rationale: the milestone's own instruction is to pick the harness whose discovery was most automatic, and no harness surfaced the six procedures at startup, so the pick rests on the route the three records actually distinguish. In Claude Code the bootstrap procedure was read in the session's second tool call with no remark of any kind; omp spent a clause acknowledging that the copy step was already satisfied, and Codex CLI named the procedure it had chosen before following it. Least deliberate is furthest to fall, which is what the failing case wants. Two lesser reasons point the same way: it is the harness whose native procedure support is strongest, which is why M2 ran it first, and its bootstrap is the cheapest of the three to drive.
  Date/Author: 2026-09-21, M6 execution session.

- Decision: `blueprint-eval` stays `specced`, and both halves of the shortfall are recorded rather than the more convenient one.
  Rationale: the promotion condition this milestone carries is a conjunction — all five observations in all three harnesses, and both failing cases demonstrated — and neither half holds. Observation 3 failed in Claude Code and in omp, and M8 repaired the root cause of the first of those without re-driving the trial that would show it repaired, which is a fix with no end-to-end evidence behind it. The harness failing case did not fail. Promoting on the driver's two cases alone would record `built` for an invariant three quarters of which is a person driving a harness, which is the manufactured green check the card format warns about, and it would do it on the strength of a case that this milestone has just observed cannot fail in any harness that reads the tree.
  Date/Author: 2026-09-21, M6 execution session.

- Decision: (routed to M7) the card's failing case is left standing as written, and the finding that it cannot fail in a tree-reading harness is routed to close-out rather than repaired here.
  Rationale: the case is a specification clause on `docs/capabilities/blueprint-eval.md`, and rewriting it is a judgement about what the trial's layout half is for, not a correction of a defect: the case still does exactly what a failing case must, since the driver's layout part exited 1 and named the missing file. What the run revealed is narrower and belongs with `D4` — every harness reached the procedures through the tree, so a filename the loaders never read cannot be load-bearing for discovery in any of them, and the entry-point convention is therefore a driver-decided invariant rather than a harness-decided one. M7 has `D4` open and owns the routing; widening this milestone into a card edit would spend the status decision's own evidence on a change nobody has trialled.
  Date/Author: 2026-09-21, M6 execution session.

## Outcomes & Retrospective

The plan-level retrospective is written at M7, and it owes the reader a comparison against the purpose stated above: whether a copy of the payload and the procedures, with no access to this repository, carried one feature to an executed milestone in each of the three harnesses; which of the card's five observations held in which harness; what the trial found that reading the payload could not have found; what was fixed in the payload as a result, and what was routed to `docs/DEBT.md` instead; and what the three harnesses reported about how they found the procedures, which is the input `D4` has been waiting for. All three harness records now sit below, the third added by M5's resumed session once the account that had refused it answered again.

The one comparison worth stating before M7 writes the rest: the third harness is the only one whose copy ends the trial with every reference resolving, and it got there without this repository doing anything the other two did not also receive. What differs is the target project's own conduct — it wrote its reserved paths into a debt row, then deleted the row in the milestone that created the paths.

### Claude Code

Harness `2.1.274 (Claude Code)`, 2026-09-17, in the trial repository `$HOME/blueprint-trials/claude-code-20260917-152930`. Three sessions, each a separate non-interactive process with no `--continue` and no `--resume`, each invoked as `claude -p --model opus --dangerously-skip-permissions` with a brief from `logs/` and standard input closed, each exiting 0: bootstrap under M2 at 4 minutes 57 seconds, plan authoring at 8 minutes 3 seconds, milestone execution at 4 minutes 35 seconds. The copy ends the trial with a working `tally`: `python3 -m tally add` and `python3 -m tally report` run, `./verify` is published and passes, and seven tests exist where at bootstrap there was no code at all. Four of the card's five observations hold; the third does not, and its shortfall is a single reference.

Observation one — the harness discovered every skill with no per-harness edit to any file — holds under the criterion the card carries, and it holds by way of the tree rather than the harness. The bootstrap session listed the files and read the bootstrap procedure in its second tool call, four minutes before it created the symlink that makes the procedures harness-visible, and this machine's own skills directory holds none of the six. That is the `D4` evidence, recorded in full by M2.

Observation two — after bootstrap, the fill check over the copy's skeleton-derived artifacts comes back empty — holds: `fill: ok — no authoring scaffolding in 23 markdown files of the copy.`

Observation three — every path reference inside the copy resolves inside the copy — fails, with one reference outstanding after the executed milestone: `references: 1 dangling reference in 161 examined across 21 markdown files of the copy.`, the survivor being `/tmp` cited by the copy's `docs/PRINCIPLES.md:17` while stating where the target keeps its store. It is down from 33 at bootstrap, 32 of which were the reserved code paths the executed milestone created. The shortfall belongs to the reference definition rather than to the target project: it extracts an absolute system path as a repository-relative reference, which this repository has now seen twice in two different trees, and the fix is a change to `docs/capabilities/doc-integrity.md` in both halves with its own acceptance.

Observation four — the executed milestone's living sections carry output observed in that session rather than restated from the plan — holds, judged from git as this milestone's acceptance requires. The diff of the copy's plan file between the authoring commit and the execution commit adds `./verify  0.11s user 0.04s system 30% cpu 0.495 total`, a line the authoring version cannot contain and does not.

Observation five — the session stopped after one milestone rather than continuing — holds: one milestone ticked with its own nine steps beneath it, four milestones open, four commits all prefixed `M1:`, and no file that a later milestone creates present in the copy.

What M8 re-ran here, 2026-09-21. The bootstrap half was driven again against the fixed payload in a fresh trial, `$HOME/blueprint-trials/claude-code-postfix-20260921-152808`, harness `2.1.274 (Claude Code)`, 4 minutes 55 seconds, exit 0, commit `2b2691b` over `da09bea Receive the payload`. Fill came back empty over 23 markdown files as before, and the references part reported 20 dangling citations, all of them `tally/core/`, `tally/store.py` or `tally/cli.py`, with no absolute path among them — where the pre-fix bootstrap of this harness produced 33 citations of which one was the `/tmp` mention that later cost observation 3. The session reported the reserved class by name instead of reporting its run as not clean, and wrote the `docs/DEBT.md` row the fixed step asks for. The authoring and execution halves were not re-driven, because the fix touches neither procedure: observations 3, 4 and 5 above stand as recorded against the pre-fix trial. Observation 3's shortfall is narrower than it was — the definition no longer extracts the citation that survived it — but nothing post-fix has carried a milestone to execution in this harness, so it is not re-observed and is not claimed.

### omp

Harness `omp/18.1.14`, 2026-09-17, in the trial repository `$HOME/blueprint-trials/omp-20260917-170102`. Three sessions, each a separate non-interactive process with no resumption, each invoked as `omp -p --auto-approve --cwd <trial>/repo` with a brief from `logs/` and standard input closed, each exiting 0: bootstrap at 6 minutes 29 seconds, plan authoring at 13 minutes 14 seconds, milestone execution at 9 minutes 14 seconds — 29 minutes of driven work against Claude Code's 18. The `--auto-approve` flag grants the harness permission to act and is recorded with the invocation, which is what the card's own sentence about permission flags requires; standard input was closed for the same reason it is closed in the other harness, because a driver has none to give. The copy ends the trial with a working `tally`: `python3 -m tally add build` twice, `add ship` and `report` printed `build 2` then `ship 1` and exited 0, the store landing at `.tally-store` inside the checkout and `TALLY_STORE` moving it; the project's own published command `./check` printed `Ran 36 tests in 0.334s`, `OK` and exit 0, where at bootstrap there was no code and no command at all. Four of the card's five observations hold; the third fails, with one citation outstanding, and it fails for a different reason than it failed in Claude Code. Seven commits: the payload, the bootstrap, the authored plan, and four prefixed `M1:`.

Observation one — the harness discovered every skill with no per-harness edit to any file — holds under the criterion the card carries, and this harness is where the criterion earns its wording. omp has a skills mechanism and it surfaced none of the six: a read-only probe in a fresh copy reported twenty skills registered from the machine owner's own roots and stated that the copy's procedures were not among them, because the copy carries no root the loader reads. The trial session reached `skills/harness-init/SKILL.md` four seconds after listing the tree and six minutes before it wrote anything, so the route was the tree, as in Claude Code. The same probe run in the bootstrapped copy afterwards reported all six at startup, loaded from the copy's own procedure directory, which makes the link that step 9 of the bootstrap procedure told the session to write both necessary and sufficient here. That pair is the `D4` evidence this harness owed: the procedures are not in omp's discovery path on arrival, one link puts them there, and the session had to choose that link's location itself.

Observation two — after bootstrap, the fill check over the copy's skeleton-derived artifacts comes back empty — holds: `fill: ok — no authoring scaffolding in 22 markdown files of the copy.` One file fewer than the Claude copy, because this environment reads `AGENTS.md` itself and the session correctly wrote no second guide.

Observation three — every path reference inside the copy resolves inside the copy — fails, with one reference outstanding after the executed milestone: `references: 1 dangling reference in 160 examined across 22 markdown files of the copy.`, the survivor being the bare core directory cited by the copy's `AGENTS.md:16`, a map line that names the tool's three layers without the package prefix. It is down from 25 at bootstrap, every one of which was a reserved code path the executed milestone created. This shortfall belongs to the target project rather than to the payload or the definition: the copy wrote a path relative to a prefix it omitted, and the reference card it kept tells it to write the path in full or carry an allowlist key with a reason. The absolute-path class that withheld this observation in Claude Code did not appear here at all, so the fix M8 lands does not carry this harness over the bar either.

Observation four — the executed milestone's living sections carry output observed in that session rather than restated from the plan — holds, judged from git. The diff of the copy's plan file between the authoring commit `4a3fb86` and the execution head `2d9c43f` is 214 insertions and 26 deletions, and among the insertions `Ran 36 tests in 0.313s` appears three times where the authoring version contains it zero times, introduced by `01161c3 M1: record the milestone's observed evidence in the plan`. The pre-fix failure line `FAILED (failures=11, errors=3)` is new in the same direction.

Observation five — the session stopped after one milestone rather than continuing — holds: one milestone entry ticked and timestamped, four open, four commits all prefixed `M1:`, and none of the later milestones' artifacts present — no aggregating check with a hook for M3, no isolation test for M4, no close-out for M5. The session's report names M2 as next and states what M2 must run first.

The clauses this milestone inherits and the rest of the record. No prompt named this repository: `grep -rc 'harness-blueprint' <trial>/logs/` printed `0` for all six files there, the three briefs and the three transcripts. The briefs were extracted from this plan's own indented blocks rather than retyped — 62, 21 and 6 lines, with no indentation left on any line — so the text the harness received is this file's text. The operator created and edited nothing inside the copy: `ls -a <trial>/repo` lists one harness configuration entry, `.omp`, and `git show --stat 47505cb` shows the session's own bootstrap commit creating it; this machine has no omp-level skills directory at all for the six to have been installed into. The copy's filled `AGENTS.md` was 58 lines at bootstrap and is 62 after the executed milestone, against the guide's own cap of about 100; its map names `skills/`; its Commands section at bootstrap said the project had no cheap verification command yet and named `D2` for it, and after M1 it publishes `./check` with a 60-second budget, which is the command that was run. The register kept `fast-verify`, `isolated-env` and `boundary-lint` with a row each, plus `evidence-check` and `doc-integrity`, five rows all `specced`; `docs/DEBT.md` carried `D1` and `D2` for the absent code and the missing command at bootstrap and drops both in the executed milestone, leaving `D3` through `D5`. The copy's plan carries thirteen `##` sections including all four the convention requires, and five `### M` milestone headings. In this repository, `./tools/verify` ended in `6 of 6 checks passed` before the trial and after it, and `git status --porcelain` reported nothing outside this plan file, whose two evidence commits are this session's only change here.

What M8 re-ran here, 2026-09-21. The bootstrap half was driven again against the fixed payload in a fresh trial, `$HOME/blueprint-trials/omp-postfix-20260921-152834`, harness `omp/18.1.14`, 5 minutes 26 seconds, exit 0, commit `e88f00c` over `b5ee65d Receive the payload`. Fill came back empty over 22 markdown files as before, and the references part reported 23 dangling citations, every one of them `tally/`, `tally/cli.py`, `tally/core/` or `tally/store.py`, with the copy's `docs/DEBT.md` `D2` naming all four and listing the eight allowlist keys a reference check built today would need. The bare `core/` citation that cost this harness observation 3 above is absent from the re-run's artifacts, but that is a different session writing a different map line rather than a fix: nothing in the payload prevents it. The authoring and execution halves were not re-driven — the fix touches neither procedure — so observations 3, 4 and 5 above stand as recorded against the pre-fix trial, and no post-fix trial in this harness has yet reached an executed milestone.

### Codex CLI

Harness `codex-cli 0.150.1`, 2026-09-21, in the trial repository `$HOME/blueprint-trials/codex-20260921-154505`, built from the payload as it stands after M8 and therefore owing no re-run. Three sessions, each a separate non-interactive process with no resumption, each invoked as `codex exec -C <trial>/repo -m gpt-5.6-sol --approve-for-me` with a brief from `logs/` and standard input closed, each exiting 0: bootstrap at 10 minutes 50 seconds committing `60ebbe6`, plan authoring at 9 minutes 55 seconds committing `74d18e5`, milestone execution at 9 minutes 21 seconds committing `bb405f3` — 30 minutes of driven work, the slowest of the three. The approval flag is the permission flag for this harness and implies its workspace-write sandbox, which the banner reports as covering the working directory, `/tmp` and `$TMPDIR`; the model flag is required because the configured default is refused by the service for this CLI version, and both are recorded with the invocation for the reason the card gives. The copy ends the trial with a working `tally`, a published verification command, and — alone among the three — a reference walk that comes back clean. All five observations hold.

Observation one — the harness discovered every skill with no per-harness edit to any file — holds, and this is the trial's cleanest instance of it. The probe pair settles the harness's own behavior: a read-only session in a fresh copy reported 24 skills from the machine owner's system and plugin roots and named none of the six, and the same prompt in the bootstrapped copy reported 14 from those same kinds of root and again named none. The copy carries no harness configuration directory at any point — `ls -a <trial>/repo` lists `.git`, `.gitignore` and the project's own directories and nothing else — so unlike the other two trials there is not even a link to argue about: the session's route was the tree, by `git ls-files` and then straight into the procedure it named. That is this harness's `D4` evidence, and the Decision Log records why the empty second clause of the install step is routed to `D4` rather than treated as a defect.

Observation two — after bootstrap, the fill check over the copy's skeleton-derived artifacts comes back empty — holds: `fill: ok — no authoring scaffolding in 22 markdown files of the copy.` Twenty-two, as in omp and one fewer than Claude Code, because this environment reads `AGENTS.md` itself and the session correctly wrote no second guide.

Observation three — every path reference inside the copy resolves inside the copy — **holds**, for the first and only time in this trial: `references: ok — 108 of 110 references resolved inside the copy, 1 allowlist entry applied, 0 stale.`, with all three parts exiting 0 together. At bootstrap the part reported 6 dangling citations, every one a reserved `tally/` path named by the `docs/DEBT.md` row the fixed procedure asks for; the executed milestone created the package, paid both bootstrap debt rows and deleted them, and the citations went with the row that carried them. The two references the summary excludes are the copy's own `doc-integrity` card naming the path its failing case creates, which is the seeded allowlist key and the same one every trial has applied.

Observation four — the executed milestone's living sections carry output observed in that session rather than restated from the plan — holds, judged from git. The plan file gains 18 lines between `74d18e5` and `bb405f3`, among them `real 0.37` and `Ran 1 test in 0.099s`, each appearing zero times in the authoring version, and an `AssertionError: 1 != 0` quoted from the red run. The counter-example recurs too: `fast-verify: FAIL (unittest)` was already in the authoring version twice as a prediction.

Observation five — the session stopped after one milestone rather than continuing — holds: two ticked entries, both Milestone 1's, one for its red check and one for the milestone itself, two milestones open, one commit implementing the slice, and none of the later milestones' work present — no label validation, no atomic replacement, no isolation test, no behavior spec beside `docs/specs/index.md`. The closing report names Milestone 2 as next and says it was not started.

The clauses this harness inherits, and the rest of the record. No prompt named this repository: `grep -rc 'harness-blueprint' <trial>/logs/` printed `0` for all seven files there, the three briefs, the three transcripts and the stored probe. The briefs were extracted from this plan's own indented blocks at 62, 21 and 6 lines with no indentation left on any line. The operator created and edited nothing inside the copy. The filled `AGENTS.md` was 47 lines at bootstrap, its map names `skills/`, and its Commands section said that no verification command exists yet and named the debt row tracking it; by the end of the executed milestone that line reads `python3 scripts/verify.py`, and the register carries `fast-verify` as `built` with that command as its enforcement point. The copy's store is `.tally.json` inside the checkout with `TALLY_STORE` overriding it, and `add build` twice, `add ship` and `report` print `build` and `ship` with tab-separated counts, exit 0.

One departure from what this plan expected of a first milestone, recorded rather than smoothed over: the suite behind that command is a single end-to-end test, which drives `add` three times through a subprocess and asserts the exact two-line report, where M3's acceptance anticipated at least one direct test of the counting logic. Counting is covered through the command rather than under it, and the copy's own plan says so — unit tests of the pure functions and the store sit in its Milestone 2, along with label rejection and atomic replacement. Against the card's five observations nothing here fails; against the shape of a first slice, this harness bought less test surface than the other two, which landed seven and thirty-six tests respectively.

### The failing cases, and the status

All three failing cases this plan owes are now observed, and they did not come back the same. The driver's two, run in M1: a deleted payload file produced ten dangling citations where the acceptance clause expected one, and a renamed procedure produced a layout violation naming both the expected path and what arrived instead. The harness case, run in M6 under the same rename: the harness bootstrapped the copy regardless, in 457 seconds, and the only thing that reported the defect was the driver.

That is why `docs/capabilities/index.md` still reads `specced` for this card. Two things withhold the promotion and both are named here rather than the easier one alone. Observation 3 failed in two of the three harnesses — the absolute-path citation in Claude Code, whose cause M8 removed from the definition but whose trial was never re-driven end to end, and the bare core directory in the omp copy's own map line, which belongs to that target project. And the harness failing case did not fail, so the promotion bar's question — has anyone seen this check catch the violation it exists to catch — has an answer for the scriptable quarter and no answer at all for the other three.

What would earn `built` is therefore two things a later plan can do and this one cannot: one end-to-end trial per harness against the payload as it now stands, of which Codex CLI's is already in hand, and a failing case for the harness-driven part that a harness can actually fail. The second is the harder one, and M6's evidence says why: all three harnesses reached the procedures by reading the tree, so no filename convention is load-bearing for discovery in any of them, and a case built on renaming one will keep coming back clean.

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

Eight milestones. M1 builds what the trial needs and changes no claim. M2 and M3 run the first harness. M4 and M5 run the other two. M6 demonstrates the failing case that gates promotion and sets the status. M8, appended by M2 when the first harness turned up a payload defect, lands that fix and re-runs whatever it invalidates. M7 routes everything to its owner and closes the plan, and it runs last whatever number stands in front of it.

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
2. `./tools/blueprint-eval check <trial>/repo` exits zero with all three parts passing: the layout part finds six procedures, the fill part finds no marker anywhere in the copy, and the references part resolves every reference inside the copy. This is observations 2 and 3, and the exact summary lines go into the plan. Amended by M2's execution, which observed that the references part cannot pass on a copy whose code does not exist yet: the references half of this clause moves to M3, which re-runs the part after the executed milestone lands the package, and this milestone instead records the composition of what dangles. The Decision Log entry on judging that observation at the end of a trial owns the reasoning, and M4 and M5, which inherit these clauses, inherit the amendment with them.
3. The discovery record for Claude Code is in this plan: the exact invocation line used; the harness version from `claude --version`; the date; whether any file was created or edited outside `repo/`'s own artifacts to make the procedures visible, evidenced by `ls -a <trial>/repo` showing no harness configuration directory that the copy did not arrive with; and a quoted line or two from the transcript showing how the session named the procedure it followed. This is observation 1 under the criterion M1 wrote onto the card, and it is the evidence `D4` is waiting for.
4. No prompt named this repository: `grep -rc 'harness-blueprint' <trial>/logs/` reports zero for every brief file, and the brief files are the ones this plan specifies verbatim.
5. The copy's filled `AGENTS.md` is under about 100 lines, its map has a line naming `skills/`, and its Commands section says honestly that the project has no verification command yet — there is no code at bootstrap, and `skills/harness-init/SKILL.md` forbids publishing a command nobody ran. The copy's `docs/capabilities/` keeps `fast-verify`, `isolated-env` and `boundary-lint`, each with a register row, and `docs/DEBT.md` in the copy carries at least the missing-verification-command row.
6. `./tools/verify` in this repository still passes, and `git status --porcelain` here reports nothing outside this plan file — the trial mutates nothing in this tree, and the plan's own evidence commits are the session's only change here.

The work. Build the trial repository with `./tools/blueprint-eval new claude-code`. Write the three briefs from Interfaces and Dependencies into `<trial>/logs/brief-bootstrap.md`, `<trial>/logs/brief-author.md` and `<trial>/logs/brief-execute.md`, verbatim, with no path to this repository anywhere in them; they live in `logs/` rather than in `repo/` so that they are not artifacts of the project under test. Run the bootstrap session with the invocation under Concrete Steps, capturing the transcript to `<trial>/logs/01-bootstrap.txt`. Then run the driver's three parts and read the copy.

Record what the session did with the two things authoring found. `skills/harness-init/SKILL.md`'s Locate step tells it to copy the artifacts from the checkout it was invoked from, which does not exist here; the brief tells it the copy arrived already done, and the milestone records whether that read as followable or as a contradiction the session had to work around. And the payload's map cannot name the procedures, so record how the session found them in the absence of a map line.

If the session leaves a marker in a filled artifact, a dangling reference, or an artifact it invented rather than filled, that is a finding, not a failure of the milestone: record it, decide whether it is a defect in the payload or in the procedure, and fix it where it belongs, in both halves when the defect is in the payload. If a fix is bigger than the remaining session, split the milestone in place in `Progress`, log the split, and stop.

### M3 — Claude Code, plan and execution half

Two more driven sessions in the same trial repository: one authors the target's first plan, one executes its first milestone. At the end, `tally` runs, and observations 4 and 5 are recorded for Claude Code.

Acceptance, seven observable checks. The seventh was moved here from M2 by M2's session, whose record explains why.

1. A plan file exists under `<trial>/repo/plans/active/`, carries all four required living sections — `Progress`, `Surprises & Discoveries`, `Decision Log`, `Outcomes & Retrospective` — and describes at least two milestones, of which the first delivers `tally add` and `tally report` end to end.
2. In `<trial>/repo`, `python3 -m tally add build && python3 -m tally add build && python3 -m tally add ship && python3 -m tally report` prints one line per label with counts, `build` above `ship`; the exact output goes into this plan. The store file the run created is inside the checkout, at `.tally/store.tsv` unless the project documented another path. Then `TALLY_STORE=$(mktemp -d)/s.tsv python3 -m tally report` reads that empty path instead and reports none of the labels above, which is what makes the store path an override rather than a default nobody can move.
3. The project's own verification command, as published in the copy's `AGENTS.md`, runs and passes; record the command and its output. Expect a `python3 -m unittest` invocation with at least one real test of the counting logic.
4. Observation 4, judged from git rather than from prose: the diff of the plan file between the authoring commit and the execution commit adds at least one line of recorded output that does not appear in the authoring version — the test summary, the `report` output, or the store path. Quote the added line and state which commit introduced it. Output restated from the plan's own expectations fails this observation, and a failure here is recorded, not smoothed over.
5. Observation 5: the plan's `Progress` shows exactly one entry ticked and timestamped, the later milestones still open, and `git -C <trial>/repo log --oneline` shows no commit implementing a later milestone's scope. If the session continued past one milestone, record it as a failure of this observation and name what it did.
6. The outcome line for Claude Code is in this plan: harness, version, date, and pass or fail for each of the five observations, with the shortfall named for any that failed.
7. Observation 3, deferred here from M2: `./tools/blueprint-eval check <trial>/repo references` is run again after the executed milestone has landed the package, and its summary line goes into this plan. The reserved paths the bootstrap left dangling — `tally/core/`, `tally/store.py`, `tally/cli.py` — now exist, so what remains is the honest residue: whatever the copy still cites and does not contain. Name each remaining one and say whose it is — a target project's deliberate mention, which belongs in its own allowlist and in its debt row, or a payload defect, which belongs in this repository. A part that still exits 1 is recorded as the shortfall it is, against this clause.

The work. Run the authoring session, then the execution session, as two separate invocations with no resumption of the first — the card requires the execution to happen in a fresh session, and a separate process invocation with no `--continue` or `--resume` is what makes that observable. Capture transcripts to `<trial>/logs/02-author.txt` and `<trial>/logs/03-execute.txt`. Between the two, commit nothing in this repository except this plan.

The evidence discipline that binds this session binds the one under test too, and the asymmetry is worth holding in mind: this milestone's job is to observe what the driven session recorded, not to improve it. Do not edit the trial repository by hand to make an observation pass.

### M4 — omp, whole trial in one session

Everything M2 and M3 did, in one session, against a fresh trial repository built for omp. The payload has already been through one full trial, so this session's expected cost is the three driven sessions plus the evidence.

Acceptance: the same checks as M2 items 1 through 6 and M3 items 1 through 6, against a trial directory built with `./tools/blueprint-eval new omp`, with the harness version from `omp --version` and the invocation under Concrete Steps. Two additions specific to this harness. Record whether omp surfaced the procedures automatically and, if its output says so, from which root and under what configuration — `omp --help` documents `--no-skills` and `--skills=<glob>`, so the mechanism exists and what it scans is exactly the `D4` question. And record whether the session was run with `--auto-approve`; a permission flag is not a file edit and does not violate observation 1, but it is part of the invocation and belongs in the record.

If this session finds a payload defect, the Decision Log's re-run rule applies: fix it in this repository, and append a re-run milestone for Claude Code, whose result the fix invalidates.

### M5 — Codex CLI, whole trial in one session

The same again for Codex CLI, whose help text names no skills mechanism at all. M5's first session found that the help text is not the machinery: the mechanism exists in `codex-cli 0.150.1`, it is enabled by default, and its default root map includes a workspace-relative procedure directory, so whether this harness surfaces the copy's six procedures by itself is open rather than settled against it. The `Surprises & Discoveries` entries from that session carry the evidence, and a Decision Log entry records why the framing was corrected here rather than left to be tripped over.

Acceptance: the same checks as M4, against a trial directory built with `./tools/blueprint-eval new codex`, with the harness version from `codex --version` and the invocation under Concrete Steps. Three additions. Record how the session reached the bootstrap procedure, quoting the transcript, and settle whether the harness surfaced the procedures at startup the way M4 settled it in omp — a read-only probe in a fresh copy and another in the bootstrapped copy, since a print-mode transcript does not say what was registered into the session and a session may read a file it was already given. Record the sandbox and approval flags used, and the model flag: `--approve-for-me` implies the workspace-write sandbox and refuses to be paired with `-s`, the sandbox denies network access, which the dependency-free target does not need, and the configured default model is refused by the service for this CLI version, so a run that needed more permission than `--approve-for-me`, or a different model than the one recorded under Concrete Steps, is itself an observation worth the line it takes. And state explicitly whether observation 1 held under the criterion on the card, because this is the harness that criterion was written for.

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

### M8 — Land the payload fix the trial earned, and re-run what it invalidates

Appended by M2's session, which observed the defect rather than predicted it. `skills/harness-init/SKILL.md` step 10 requires that every backticked repository-relative path across the filled artifacts resolve, and its stop condition requires those checks to have come back clean, while `template/docs/capabilities/doc-integrity.md` names as its fourth legitimate class a deliberate mention of a file that does not exist, carried as an allowlist entry with a reason. A bootstrapping project states its layering as path rules before the code exists — the payload asks for exactly that — so the procedure demands a state its own card declares both unattainable and fine. The Claude Code session split the difference by reporting its run as not clean and seeding a debt row; the next session may as easily read the step literally and either weaken the layer rules until they resolve or claim a clean run it did not have.

Acceptance, five observable checks. The fifth was moved here from M7 by M3's session, which observed that the absolute-path class blocks this milestone's own third check and cannot wait for close-out.

1. `skills/harness-init/SKILL.md` step 10 and its stop condition name the reserved-path class and what a session does with it — a debt row naming the paths and the allowlist seeds, not a rewritten artifact and not a silent pass — in words `template/docs/capabilities/doc-integrity.md` already uses for its fourth class. The step still requires the walk to be run and its result recorded, because the finding that survives here is that a session ran it, read it, and reported honestly.
2. `./tools/verify` ends in `6 of 6 checks passed`, and `prose-duplication` still reports zero violations with no new allowlist entry: the new sentences state the class in the procedure's own words rather than restating the card's.
3. A fresh trial repository built after the fix, bootstrapped in one harness with brief one, produces a filled artifact set whose dangling references are exactly the target's reserved paths, and a `docs/DEBT.md` row naming them — observed, with the trial path and the transcript line, not inferred from the fix.
4. Every harness whose bootstrap record in this plan was made against the pre-fix text has that record either re-run against the fixed text, or labelled in `Progress` as describing a payload that no longer exists, with the shortfall named. As of M2 that is Claude Code alone; M4 and M5 add themselves to this list if they run before this milestone.
5. The reference definition no longer treats an absolute system path as a repository-relative reference to resolve: `docs/capabilities/doc-integrity.md` states the exclusion in both halves, `tools/checks/doc-integrity` and `tools/blueprint-eval` implement it, and the references part over the trial repository of check 3 reports the target's reserved paths alone with no `/tmp` citation. A trial whose only residue is that citation is what M3 observed in Claude Code, and the exclusion is what lets check 3 above be met rather than carved out.

The work. The procedure fix is a few sentences in one file and the definition fix is one span-exclusion rule stated on a card and implemented in two scripts; the whole risk in both is scope, and neither is an invitation to revisit step 10's other clauses, the card's four classes, or the driver's strict resolution, all of which the trial found working. The re-run obligation is the Decision Log's rule applied to this plan's own record, and it is why this milestone sits before close-out rather than after it.

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

The second finding M2 routed here — the reference definition extracting an absolute system path as a reference to resolve, seen in this repository during M1 and again in the trial copy where `docs/PRINCIPLES.md` cites `/tmp` — has moved to M8 as its fifth acceptance check, by the decision M3 logged. It left because it is a change under `template/`, which the re-run rule turns into a re-driven bootstrap, and a close-out milestone cannot both invalidate the harness records above it and close the plan over them. What remains here is the verification: confirm the exclusion landed, that no `/tmp` citation survives in M8's trial, and that no allowlist entry was seeded in its place.

## Concrete Steps

Every command below runs from this repository's root unless a working directory is named. `<trial>` stands for the path `./tools/blueprint-eval new` printed, which the milestone's `Progress` entry records so the next session can find it. The driver's two transcripts below were observed on 2026-09-17 during M1, with the trial path of that run. Every harness invocation further down is observed: the three Claude Code lines during M2 and M3, the three omp lines and the omp discovery probe during M4, all on 2026-09-17, and the three Codex CLI lines and its probe during M5's resumed session on 2026-09-21. The comments under each line carry what that run printed, including the refusals a harness produced before it would run at all.

Build a trial repository and check it.

    ./tools/blueprint-eval new claude-code
    # observed, with the label smoke, 2026-09-17:
    # blueprint-eval: trial at /Users/<you>/blueprint-trials/smoke-20260917-151702
    # blueprint-eval:   repo  /Users/<you>/blueprint-trials/smoke-20260917-151702/repo
    # blueprint-eval:   logs  /Users/<you>/blueprint-trials/smoke-20260917-151702/logs
    # blueprint-eval: copied 25 files — 19 payload files and 6 procedures — committed as "Receive the payload".
    # git -C <trial>/repo log --oneline then showed one commit: dbc343a Receive the payload
    # ls <trial>/repo listed AGENTS.md ARCHITECTURE.md GOALS.md docs plans skills

    ./tools/blueprint-eval check <trial>/repo
    # observed on that fresh, unfilled copy: layout and references pass, fill fails, so the run exits 1.
    # blueprint-eval layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.
    # blueprint-eval fill: AGENTS.md:1 still carries authoring scaffolding.
    #   A fill slot or a guidance block survived the bootstrap, so this artifact is a skeleton the project has not answered yet. …
    #   … 116 further locator lines, one per marker line, the paragraph above repeated once per artifact …
    # blueprint-eval fill: 117 marker lines in 8 of 23 markdown files.
    # blueprint-eval references: ok — 144 of 146 references resolved inside the copy, 1 allowlist entry applied, 0 stale.

Refusals, both observed: `./tools/blueprint-eval check .` exits 2 with `cannot decide — /…/harness-blueprint is inside this repository's worktree.`, and `check` on a path that is not a directory exits 2 the same way. Each prints the reason a trial runs outside this tree and how to build one.

Run the three driven sessions. Each is a separate process with no resumption, which is what makes "a fresh session" observable. The brief files are written from this plan before the first one runs.

    cd <trial>/repo && claude -p --model opus --dangerously-skip-permissions "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    cd <trial>/repo && claude -p --model opus --dangerously-skip-permissions "$(cat ../logs/brief-author.md)"    < /dev/null 2>&1 | tee ../logs/02-author.txt
    cd <trial>/repo && claude -p --model opus --dangerously-skip-permissions "$(cat ../logs/brief-execute.md)"   < /dev/null 2>&1 | tee ../logs/03-execute.txt
    # all three of these are observed, 2026-09-17, in claude-code-20260917-152930. The bootstrap
    # ran 4 minutes 57 seconds, exited 0, and committed ae00403 "Bootstrap the agent harness for tally";
    # the authoring line ran 8 minutes 3 seconds, exited 0, and committed c82847f, 881 lines of plan
    # and nothing else; the execution line ran 4 minutes 35 seconds, exited 0, and committed four
    # times, fa2d92f through 0110215, all prefixed "M1:". The two other harnesses are still expected.
    # --model opus and < /dev/null are both M2 findings rather than authoring choices: the same line
    # without them exited 0 in 5.7 seconds having done nothing but print that the configured default
    # model requires usage credits, and having warned that it waited 3 seconds for standard input.
    # The Decision Log records why a model flag is recorded with the invocation and not counted
    # against the discovery observation.

    cd <trial>/repo && omp -p --auto-approve --cwd <trial>/repo "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    # and the same shape for the authoring and execution briefs. All three are observed,
    # 2026-09-17, in omp-20260917-170102: the bootstrap ran 389 seconds, exited 0 and committed
    # 47505cb; the authoring line ran 794 seconds, exited 0 and committed 4a3fb86, 931 lines of
    # plan and nothing else; the execution line ran 554 seconds, exited 0 and committed four
    # times, 5bace42 through 2d9c43f, all prefixed "M1:". Standard input is closed for the same
    # reason it is closed in the other harness; --auto-approve is the permission flag, recorded
    # with the invocation. Nothing else was passed: no model flag, no skills flag.

    cd <probe>/repo && omp -p --auto-approve --cwd <probe>/repo "Answer in plain text and touch no file: list every skill name that this session was given at startup, and for each one name the directory it was loaded from. If none were given, say so explicitly." < /dev/null
    # the discovery probe, observed twice on 2026-09-17: in a fresh copy built with the label
    # omp-skills-probe it ran 33 seconds and reported 20 skills from the machine owner's own
    # roots and none of the copy's six; in the bootstrapped trial copy it ran 63 seconds and
    # reported 26, the six among them, from the copy's own skills directory. Read-only: git
    # status --porcelain in the copy printed nothing after it.

    codex exec -C <trial>/repo -m gpt-5.6-sol --approve-for-me "$(cat <trial>/logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee <trial>/logs/01-bootstrap.txt
    # and the same shape for the authoring and execution briefs. All three are observed,
    # 2026-09-21, in codex-20260921-154505: the bootstrap ran 650 seconds, exited 0 and
    # committed 60ebbe6; the authoring line ran 595 seconds, exited 0 and committed 74d18e5,
    # 232 lines of plan and nothing else; the execution line ran 561 seconds, exited 0 and
    # committed once, bb405f3, carrying the whole first milestone. Standard input is closed
    # for the same reason it is closed in the other two harnesses.
    # The flags, both recorded with the invocation rather than counted against discovery:
    # --approve-for-me is the permission flag and implies the workspace-write sandbox, which
    # the banner reports as "sandbox: workspace-write [workdir, /tmp, $TMPDIR]" with
    # "approval: on-request"; passing -s beside it exits 2, which is what M5's first session
    # found. The model flag is required because the configured default, gpt-6-astra, is
    # refused by the service for this CLI version, exit 1 in 3.9 seconds — still true on
    # 2026-09-21. The account-level usage limit that blocked M5's first session has expired:
    # gpt-5.6-sol, gpt-5.6-luna and gpt-5.5 each answered a one-word probe, exit 0.
    # One refusal costs nothing and is worth knowing: codex exec in a directory that is not
    # a git repository exits 1 in 0.17 seconds with "Not inside a trusted directory and
    # --skip-git-repo-check was not specified", before any model call. The driver commits
    # the copy, so a trial repository never hits it.

    codex exec -C <probe>/repo -m gpt-5.6-sol --approve-for-me "Answer in plain text and touch no file: list every skill name that this session was given at startup, and for each one name the directory it was loaded from. If none were given, say so explicitly." < /dev/null
    # the same discovery probe, observed twice on 2026-09-21: in a fresh copy built with the
    # label codex-skills-probe it ran 49 seconds and reported 24 skills, five from the
    # machine owner's system skills directory and nineteen from plugin caches, and none of
    # the copy's six; in the bootstrapped trial copy it ran 23 seconds and reported 14 from
    # those same roots, again none of the six. Read-only: git status --porcelain printed
    # nothing in either copy afterwards. Unlike omp, this harness's bootstrap wrote no link
    # for the second probe to find, so the pair reports the harness alone.

If a harness refuses to run non-interactively, cannot authenticate, or stops on a prompt no flag answers, that is a finding: record what it printed, then run that harness's sessions interactively in the same trial repository with the same briefs pasted in, and say in the record that the session was driven by hand. If it cannot be driven at all, the trial is incomplete for that harness, the card stays `specced`, and M6 records which harness withheld the promotion. Do not substitute a different harness.

Read the result.

    ./tools/blueprint-eval check <trial>/repo
    # observed after the Claude Code bootstrap, 2026-09-17, exit 1 on the references part:
    # blueprint-eval layout: ok — 6 procedures, each with SKILL.md and matching frontmatter name.
    # blueprint-eval fill: ok — no authoring scaffolding in 23 markdown files of the copy.
    # blueprint-eval references: 33 dangling references in 149 examined across 21 markdown files of the copy.
    #   32 of them cite tally/core/, tally/store.py or tally/cli.py, the layering the brief prescribed
    #   and the code the first milestone has not written yet; the 33rd cites /tmp from docs/PRINCIPLES.md:17.
    # observed again after the executed milestone, 2026-09-17, still exit 1 but with one citation left:
    # blueprint-eval references: the copy cites /tmp, which the copy does not contain (cited by docs/PRINCIPLES.md:17).
    # blueprint-eval references: 1 dangling reference in 161 examined across 21 markdown files of the copy.
    # the same two runs in omp-20260917-170102, 2026-09-17, both exit 1 on the references part:
    # blueprint-eval fill: ok — no authoring scaffolding in 22 markdown files of the copy.
    # blueprint-eval references: 25 dangling references in 144 examined across 20 markdown files of the copy.
    #   all 25 cite tally/, tally/cli.py, tally/core/ or tally/store.py; no absolute path is cited at all.
    # blueprint-eval references: the copy cites core/, which the copy does not contain (cited by AGENTS.md:16).
    # blueprint-eval references: 1 dangling reference in 160 examined across 22 markdown files of the copy.
    # The two copies publish different names for the same behavior: the omp copy's verify command is
    # ./check over 36 tests and its store is .tally-store, where the Claude copy's are ./verify and
    # .tally/entries.log. Both answer add and report identically: build 2 then ship 1, exit 0.
    # the same two runs in codex-20260921-154505, 2026-09-21: the first exits 1, the second exits 0:
    # blueprint-eval references: 6 dangling references in 114 examined across 20 markdown files of the copy.
    #   all 6 cite tally/core/, tally/store.py or tally/cli.py, and the copy's docs/DEBT.md D2 names
    #   them as allowlist keys; after the executed milestone paid and deleted that row:
    # blueprint-eval references: ok — 108 of 110 references resolved inside the copy, 1 allowlist entry applied, 0 stale.
    # That is the only clean reference walk in the trial, and the third set of names for one behavior:
    # this copy's store is .tally.json, its published command is python3 scripts/verify.py, and its
    # report prints build and ship with tab-separated counts, exit 0.
    cd <trial>/repo && git log --oneline
    # observed after all three Claude Code sessions: b55f463 Receive the payload, ae00403 Bootstrap the
    # agent harness for tally, c82847f Author the ExecPlan, then fa2d92f, 2f3ba4b, a3df528 and 0110215, all M1.
    cd <trial>/repo && python3 -m tally add build && python3 -m tally add build && python3 -m tally add ship && python3 -m tally report
    # observed, two lines and exit 0: build 2 then ship 1. The store is .tally/entries.log inside the
    # checkout; TALLY_STORE=$(mktemp -d)/s.tsv python3 -m tally report then printed nothing, exit 0.
    cd <trial>/repo && ./verify
    # observed: verify: running tests / Ran 7 tests in 0.112s / OK / verify: 1 check passed (tests). — exit 0, 0.359s real.
    cd <trial>/repo && grep -n '^## ' plans/active/*.md
    # observed: the copy's plan carries all four required living sections and five milestone headings.
    cd <trial>/repo && git show --stat HEAD
    ls -a <trial>/repo
    grep -rc 'harness-blueprint' <trial>/logs/

Re-run a bootstrap after a payload fix, which is what the Decision Log's re-run rule costs. Both lines below were observed on 2026-09-21 during M8, each against its own fresh trial built from the fixed payload, each a separate process with standard input closed and no resumption.

    ./tools/blueprint-eval new claude-code-postfix
    sed -n '<brief-one-first-line>,<brief-one-last-line>p' plans/active/blueprint-live-trial.md | sed 's/^    //' > <trial>/logs/brief-bootstrap.md
    # observed: 62 lines, no line left indented, and grep -c 'harness-blueprint' printed 0.
    cd <trial>/repo && claude -p --model opus --dangerously-skip-permissions "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    # observed: 4 minutes 55 seconds, exit 0, one commit 2b2691b over da09bea Receive the payload.
    # ./tools/blueprint-eval check <trial>/repo then printed, exit 1 on the references part:
    # blueprint-eval fill: ok — no authoring scaffolding in 23 markdown files of the copy.
    # blueprint-eval references: 20 dangling references in 136 examined across 21 markdown files of the copy.
    #   all 20 cite tally/core/, tally/store.py or tally/cli.py; no absolute path is cited anywhere.

    cd <trial>/repo && omp -p --auto-approve --cwd <trial>/repo "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    # observed in omp-postfix-20260921-152834 against the identical brief: 5 minutes 26 seconds,
    # exit 0, one commit e88f00c over b5ee65d Receive the payload, against the 6 minutes 29 of the
    # pre-fix run. ./tools/blueprint-eval check <trial>/repo then printed, exit 1 on references:
    # blueprint-eval fill: ok — no authoring scaffolding in 22 markdown files of the copy.
    # blueprint-eval references: 23 dangling references in 136 examined across 20 markdown files of the copy.
    #   all 23 cite tally/, tally/cli.py, tally/core/ or tally/store.py; the bare core/ citation the
    #   pre-fix omp trial ended with is absent, and so is any absolute path.

Demonstrate the failing cases (M1 for the driver, M6 for the harness), and restore immediately in both directions. The two driver cases were run on 2026-09-17 and the results below are observed; the harness case was run on 2026-09-21 and is the third block, where what was observed is that the harness did not fail.

    rm template/docs/PRINCIPLES.md
    ./tools/blueprint-eval new dangling-map && ./tools/blueprint-eval check <trial>/repo references
    # observed: new printed "copied 24 files — 18 payload files and 6 procedures", and the
    # references part exited 1 with ten blocks, the first of them:
    # blueprint-eval references: the copy cites docs/PRINCIPLES.md, which the copy does not contain (cited by AGENTS.md:49).
    #   Fix this in this repository — in template/ and its live counterpart, or in skills/, whichever owns the defect — and never in the copy: the copy is a scratch artifact and the next trial rebuilds it.
    #   If the mention is deliberate, add the key AGENTS.md:docs/PRINCIPLES.md to tools/allow/blueprint-eval.txt with a reason.
    # blueprint-eval references: 10 dangling references in 143 examined across 20 markdown files of the copy.
    git checkout -- template/docs/PRINCIPLES.md && ./tools/verify
    # observed: git status clean apart from the two new untracked driver files, a fresh copy
    # back to "references: ok — 144 of 146", and verify ending in "6 of 6 checks passed".

    git mv skills/harness-init/SKILL.md skills/harness-init/README.md
    ./tools/blueprint-eval new missing-skill && ./tools/blueprint-eval check <trial>/repo layout
    # observed: new printed "copied 25 files — 19 payload files and 5 procedures", and the
    # layout part exited 1 with:
    # blueprint-eval layout: skills/harness-init/SKILL.md is missing from the copy.
    #   found skills/harness-init/README.md instead.
    #   Every supported harness reads the same entry-point filename, so the layout is fixed by all of them at once: restore the name in this repository rather than adapting one harness.
    #   Fix this in this repository — in template/ and its live counterpart, or in skills/, whichever owns the defect — and never in the copy: the copy is a scratch artifact and the next trial rebuilds it.
    # blueprint-eval layout: the copy carries 5 of 6 procedures in the layout every supported harness requires.
    git mv skills/harness-init/README.md skills/harness-init/SKILL.md && ./tools/verify
    # observed: ls skills/harness-init/ shows SKILL.md alone, a fresh copy is back to
    # "layout: ok — 6 procedures", and verify ends in "6 of 6 checks passed".
    # The allowlist entry in tools/allow/doc-integrity.txt naming the README path was
    # never edited, in either direction, and every verify run in this session reported
    # "3 allowlist entries applied, 0 stale". Verify was not run while the rename stood.

    git mv skills/harness-init/SKILL.md skills/harness-init/README.md
    ./tools/blueprint-eval new missing-skill-claude
    # observed 2026-09-21: "copied 25 files — 19 payload files and 5 procedures".
    sed -n '<brief-one-first-line>,<brief-one-last-line>p' plans/active/blueprint-live-trial.md | sed 's/^    //' > <trial>/logs/brief-bootstrap.md
    # observed: 62 lines, none left indented, and grep -c 'harness-blueprint' printed 0.
    cd <trial>/repo && claude -p --model opus --dangerously-skip-permissions "$(cat ../logs/brief-bootstrap.md)" < /dev/null 2>&1 | tee ../logs/01-bootstrap.txt
    # observed: 457 seconds, exit 0, one commit 5e4401c over d161b01 Receive the payload.
    # The harness did not fail. Its second tool call read the renamed file out of the tree
    # and it bootstrapped from there; its report recorded the filename as project debt.
    ./tools/blueprint-eval check <trial>/repo
    # observed, exit 1, the layout part alone reporting the rename:
    # blueprint-eval layout: skills/harness-init/SKILL.md is missing from the copy.
    # blueprint-eval layout: the copy carries 5 of 6 procedures in the layout every supported harness requires.
    # blueprint-eval fill: ok — no authoring scaffolding in 23 markdown files of the copy.
    # blueprint-eval references: 22 dangling references in 138 examined across 21 markdown files of the copy.
    git mv skills/harness-init/README.md skills/harness-init/SKILL.md && ./tools/verify
    # observed: git status clean, verify ending in "6 of 6 checks passed" with
    # "3 allowlist entries applied, 0 stale", and a fresh copy back to "layout: ok — 6 procedures".
    # Verify was not run while the rename stood, and the allowlist was not edited in either direction.

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

The `references` part extracts, from every `*.md` under `<repo-dir>` except files whose own name ends in `_FORMAT.md` and files under `plans/active/` and `plans/completed/`, every backticked span and markdown link target containing a slash and none of space, tab, asterisk, angle bracket, brace or colon, and not beginning with a slash, ignoring fenced and indented blocks as quoted material. The leading-slash exclusion is M8's, landed after M3 observed that an absolute path cited by a filled artifact is reported as a reference that the copy does not contain; a path starting at the filesystem root names a place on the machine running the trial rather than one inside the copy, so it never enters the set and never enters the denominator. The convention document at `plans/PLANS.md` stays in scope — it is not a plan in flight — which matches the checked set `tools/checks/doc-integrity` uses. A reference resolves when it exists relative to `<repo-dir>` or relative to the citing file's own directory, and nowhere else; there is no parent-directory allowance, for the reason in the Decision Log. Deliberate exceptions live in `tools/allow/blueprint-eval.txt`, one per line as the citing path, a colon, the reference, two spaces, a `#` and a one-line reason, and the summary line reports how many applied and how many matched nothing. `docs/capabilities/doc-integrity.md` owns the reference definition; the driver reimplements it for a different root and a different checked set, and says so in its header.

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

- 2026-09-17 (M1 execution): relabelled the driver's two transcripts under
  Concrete Steps from expected to observed, with the trial path and the
  commit ids of the run, and added the observed output of both scriptable
  failing cases and of the two refusals. Reason: the section's own preamble
  said nothing there had been run, which stopped being true the moment the
  driver existed, and a later session comparing its output against an
  expectation rather than against a recorded observation cannot tell a
  regression from a prediction that was always wrong. No milestone
  boundaries, acceptance counts, or contracts changed; M1's `Progress` entry
  and the five new Surprises entries carry the rest of the evidence.

- 2026-09-17 (M2 execution): moved the references observation out of M2's
  acceptance clause 2 and into M3 as a seventh clause; appended M8, placed
  before M7 in both `Progress` and Plan of Work, to land the procedure fix
  this milestone's finding earned and re-run the bootstrap records it
  invalidates; relabelled the Claude Code bootstrap invocation under
  Concrete Steps as observed, with the `--model opus` and closed-stdin
  additions the run required; and routed the absolute-path extraction
  finding to M7 beside the empty-count one. Reason: a bootstrapped project
  states its layering as path rules before the code exists, which the
  payload asks for, so no trial could ever have passed that observation at
  bootstrap and the clause was measuring the wrong stage; the procedure
  contradicting its own reference card is a payload defect whose fix
  invalidates the record this session just wrote, which the Decision Log's
  re-run rule turns into a milestone rather than an edit. Milestone count
  changed from seven to eight and two acceptance counts moved with the
  clause; no contract under Interfaces and Dependencies changed, and the
  briefs are byte-for-byte as specified.

- 2026-09-17 (M3 execution): recorded the authoring and execution sessions of
  the Claude Code trial in `Surprises & Discoveries`, opened
  `Outcomes & Retrospective` with a per-harness record for Claude Code rather
  than leaving the section empty until close-out, ticked M3 with its carve-out
  against acceptance clause 7, relabelled all three Claude Code invocations
  under Concrete Steps as observed and filled the read-the-result block with
  what they printed, moved the absolute-path exclusion out of M7's routing
  paragraph into M8 as that milestone's fifth acceptance check, and logged two
  decisions: how observation 5 is judged, and why that fix cannot wait for
  close-out. Reason: the trial's first harness is now complete apart from one
  reference, and a section that says nothing is complete while a finished
  harness record exists would misstate the plan's own state; the exclusion
  moved because it is a change under `template/` and the re-run rule makes
  such a change invalidate the harness records a close-out milestone would be
  closing over. Milestone count unchanged at eight; M8's acceptance count
  moved from four checks to five; no contract under Interfaces and
  Dependencies changed, and the briefs are byte-for-byte as specified.

- 2026-09-17 (M5 execution): split M5 in place in `Progress` rather than
  ticking or failing it, recorded six observations about this machine's Codex
  CLI installation in `Surprises & Discoveries` under their own marker,
  corrected the Codex invocation under Concrete Steps — the sandbox flag and
  the approval flag cannot be paired, a model flag is required, and standard
  input stays closed — with the refusals it now carries as comments, corrected
  M5's own premise about the harness having no skills mechanism and pointed its
  discovery clause at the probe pair M4 used, added a paragraph to
  `Outcomes & Retrospective` saying why the third harness has no record there,
  and logged two decisions: why the milestone is split rather than recorded as
  a harness that cannot be driven, and why the premise was corrected in place.
  Reason: the harness refuses every model this account can reach until
  2026-09-20, on the interactive path as well as the driven one, so the three
  sessions this milestone exists to run could not run; the alternative endings
  both write something false — a tick over sessions nobody ran, or a permanent
  verdict about a harness drawn from one account on one day — and the plan is
  more useful to the next session carrying the refusal, the corrected
  invocation and a clean trial repository than carrying a guess. Milestone
  count unchanged at eight; no acceptance count changed, no contract under
  Interfaces and Dependencies changed, and the briefs are byte-for-byte as
  specified: the copies in the trial repository were extracted from this file's
  own blocks at 62, 21 and 6 lines.
- 2026-09-17 (post-M5 review): added one routed decision — the unparseable
  invocation as an authoring-practice lesson for the M7 retro, candidate
  owner plan-author. Reason: three defects in this plan now share the shape
  "composed behavior checked component-wise", and a lesson that has recurred
  three times inside one plan meets the materiality bar on recurrence alone.
- 2026-09-21 (M8 execution): recorded the milestone's two commits and its five
  observed acceptance checks, and made three edits beyond the living sections.
  The driver's contract under Interfaces and Dependencies now states the
  leading-slash exclusion, because the reference definition is a contract this
  plan carries and a contract that describes the pre-fix extractor would send
  the next session to the wrong rule. Concrete Steps gained the two observed
  re-run transcripts, in the section whose preamble distinguishes observed
  lines from expected ones. Both harness records under Outcomes gained a
  paragraph naming exactly what was re-driven and what still stands against the
  pre-fix payload, so that no reader takes a re-run bootstrap for a re-run
  trial. Milestone count unchanged at eight; no acceptance count changed and no
  milestone boundary moved.
- 2026-09-21 (M5 resumed): ran the three driven Codex CLI sessions the first
  attempt could not, in a fresh post-fix trial, and recorded them — Progress
  ticked with the full record and its narrative rewritten, eleven observations
  added to `Surprises & Discoveries` under their own marker, four decisions
  logged, a `Codex CLI` record added under `Outcomes & Retrospective` with the
  paragraph explaining its absence removed, and the Codex invocation and probe
  under Concrete Steps relabelled from expected to observed with what they
  printed. Reason: the account-level usage limit that blocked the first attempt
  had expired, so the milestone's own work became possible, and the plan's
  record of what it could not do had to stop standing where a result now
  exists. The stale pre-fix trial repository was abandoned and a fresh one
  built, as the Progress narrative directed, so this harness owes no re-run.
  Milestone count unchanged at eight; no acceptance count changed, no milestone
  boundary moved, no contract under Interfaces and Dependencies changed, and the
  briefs are byte-for-byte as specified: the copies in the trial repository were
  extracted from this file's own blocks at 62, 21 and 6 lines.
- 2026-09-21 (M6 execution): ticked M6 with its four observed acceptance checks
  and its carve-out against the first, added five observations under their own
  marker in `Surprises & Discoveries`, logged three decisions — the harness
  pick, the status, and the routing of the finding the case produced — added a
  `The failing cases, and the status` record under `Outcomes & Retrospective`,
  relabelled the Concrete Steps preamble now that the harness case has been
  run and added its observed transcript beside the two driver ones, and
  rewrote the Progress narrative to say which milestone is left. Reason: the
  case the card gates promotion on came back clean in the harness and failing
  in the driver, which is a result the plan had no shape for — the milestone's
  own clause anticipates it as a finding, so the record states what the
  harness did rather than reporting the milestone as unmet. `docs/capabilities/index.md`
  is deliberately untouched: the status stays `specced` by the same clause.
  Milestone count unchanged at eight; no acceptance count changed, no milestone
  boundary moved, and no contract under Interfaces and Dependencies changed.
