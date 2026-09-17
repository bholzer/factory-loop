# Build this repository's mechanical gate set: instantiate the payload's generic capability cards and make every one of them a running check

This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

This document must be maintained in accordance with `plans/PLANS.md`.

## Purpose / Big Picture

Today nothing in this repository is checked by a machine. Two commands are published under Commands in `AGENTS.md` and a person runs them when they remember; every other invariant this project claims — references resolve, no fact is stated twice, active plans carry their evidence sections, the layer map's dependency bans hold, the payload half and the live half correspond — is enforced by someone reading carefully. `docs/DEBT.md` records the consequence: at the close of v1 a hand sweep found a decision record citing a path that resolved from nowhere, and twelve pairs of artifacts sharing prose, eight of which were real single-owner violations. Both classes of defect had been in the tree for at least one milestone before anyone looked.

After this plan, a reader who has just cloned this repository can type one command, `tools/verify`, and watch six named checks decide those invariants in a few seconds, each one printing what it examined and how much of it. Introducing any of the violations the capability cards describe — a map line pointing at a file that does not exist, a sentence copied out of `docs/PRINCIPLES.md` into `AGENTS.md`, a heading deleted from `docs/capabilities/index.md`, a completed Progress entry with no timestamp, a path reference inside `template/` pointing outside it — makes that command exit nonzero and print the offender's file, line, and the next action to take. Attempting to commit such a violation with the repository's hook installed makes git refuse the commit.

Two debt items are paid by that outcome. `docs/DEBT.md` `D3` — the payload's generic capability cards are not instantiated for this project — is paid by instantiating the five applicable cards in `docs/capabilities/` and building each one. `docs/DEBT.md` `D1` — template ↔ live correspondence is checked by hand — is paid by building the check that `docs/capabilities/template-live-drift.md` already specifies, after which the hand walk recorded in that row is deleted rather than rewritten.

What an outside reader sees, concretely: `tools/verify` exists and passes; `docs/capabilities/index.md` lists eight cards of which six read `enforced` with `tools/verify` in the "Enforced at" column; `docs/DEBT.md` no longer contains `D1` or `D3`; and `docs/specs/` describes what each check decides and what it deliberately leaves to a human.

## Progress

- [ ] M1 — `tools/verify` exists, aggregates the two checks this repository runs by hand today, and is published in `AGENTS.md` with a time budget; `fast-verify` instantiated and `enforced`; pre-commit hook wired.
- [ ] M2 — `template-live-drift` built and `enforced`; all three of its comparisons demonstrated failing; `D1` deleted from `docs/DEBT.md`.
- [ ] M3 — `doc-integrity` instantiated, built and `enforced`; allowlist seeded with the three deliberate mentions this tree already contains.
- [ ] M4 — `prose-duplication` instantiated, built and `enforced`; the two known skill ↔ card pairs allowlisted with their live twins; any newly found pair routed.
- [ ] M5 — `evidence-check` instantiated, built and `enforced`; both of its failing halves demonstrated against this plan file.
- [ ] M6 — `boundary-lint` instantiated, built and `enforced`; the layer map carries the rule-id block the check binds to; the undecidable clause recorded as debt.
- [ ] M7 — close-out: budget re-measured and published, behavior reflected into `docs/specs/`, authoring decisions graduated to `docs/decisions/`, `D3` deleted, plan moved to `plans/completed/`.

Use timestamps to measure rates of progress. Nothing has been executed: this plan was authored in one sitting and no milestone has been started, so no entry above carries a completion timestamp yet.

## Surprises & Discoveries

Three things were measured while authoring, and each one changed a milestone's contract. They are recorded here because they were observed in the tree, not reasoned about.

- Observation: the completed plan `plans/completed/v1-blueprint.md` names 23 distinct markdown paths that do not exist in the tree, out of 54 distinct path-shaped references it contains.
  Evidence: extracting backticked spans ending in `.md` from that file and testing each for existence yielded 23 misses. A completed plan legitimately names files it created and later deleted, files it renamed, and illustrative paths from remediation transcripts, so a reference checker pointed at `plans/completed/` reports 23 findings and zero defects. This is why M3 fixes the checked set to the live artifact half and excludes plan files in both directories.

- Observation: the procedures under `skills/` name `docs/capabilities/index.md` seven times and `docs/specs/index.md` once, and neither path appears in the allowed-target list that `ARCHITECTURE.md` writes into its layer map.
  Evidence: extracting backticked path spans from `skills/*/SKILL.md` and comparing against the list in `ARCHITECTURE.md` under "Layer map and dependency rules" — the list names the three `docs/` directories "and their format documents", and an index file is neither a directory nor a format document. The payload does ship both index files (`template/docs/capabilities/index.md`, `template/docs/specs/index.md`), so the references are legitimate and the list is what is wrong. M6 corrects the list rather than editing eight references.

- Observation: no git remote is configured, there is no `.github` directory, and `core.hooksPath` is unset.
  Evidence: `git remote -v` prints nothing; the repository root contains only `.git`, the four root markdown files, and the `docs`, `plans`, `skills`, `template` directories. Every card in `template/docs/capabilities/` names continuous integration as half of its enforcement point, and there is no continuous integration here to name. M1 settles what the enforcement point is instead, and records the resulting weakness as debt rather than claiming a gate that does not exist.

## Decision Log

- Decision: the checks live in a new live-only directory, `tools/`, and the payload gains nothing from this plan.
  Rationale: `GOALS.md` names "no stack-specific enforcer implementations" as a non-goal, and `ARCHITECTURE.md`'s correspondence invariant runs template to live only — a live-only file is not drift. A `tools/` directory copied into `template/` would ship shell scripts that assume this repository's file set to every project that receives the payload, which is exactly the coupling the payload exists to avoid. The cards already carry the transferable part.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: one card per milestone, and `template-live-drift` is built first, immediately after the aggregator.
  Rationale: `skills/capability-build/SKILL.md` requires one card per pass, because a pass that builds two checks cannot report which one its failing case proved. Drift goes first because `docs/MATURITY.md` names it as the sole L1 gate this project has, and because its byte-identity comparison subsumes one of the two commands `AGENTS.md` publishes today, so building it early removes a published command rather than adding a second one beside it.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: the enforcement point for every check here is `tools/verify`, plus a versioned pre-commit hook at `tools/hooks/pre-commit` installed with `git config core.hooksPath tools/hooks`. The instantiated cards say that instead of naming continuous integration, and the gate's skippability is recorded as a new debt row.
  Rationale: there is no remote and no CI to run anything (see Surprises). `docs/capabilities/CARD_FORMAT.md` accepts "pre-commit, CI, or the project's cheap verification command" as `enforced`, so the status is honest; what is not honest is a card claiming a CI job nobody can observe firing. `git commit --no-verify` bypasses the hook and a fresh clone has no hook until the config line is run, which matters to the promotion rule in `docs/MATURITY.md` — a rung must not be claimed on a gate that a flag turns off. That weakness becomes debt rather than a footnote.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: `doc-integrity` decides only path references that contain a slash, only in the live artifact half, and plan files are outside its checked set in both `plans/active/` and `plans/completed/`. The payload's card gains the plan-file exclusion too.
  Rationale: the generic card says "everything under `docs/` and `plans/`", and the 23 non-resolving references in the completed plan (see Surprises) show why plan files cannot be in the set: a plan names the files it will create before they exist and the files it deleted after they are gone. That reason is universal rather than local, so under `ARCHITECTURE.md`'s rule that a generic improvement is incomplete until both halves carry it, the sentence is amended in `template/docs/capabilities/doc-integrity.md` as well. Restricting to slash-bearing references drops four bare filenames in the current tree (`PROMPTS.md`, `SKILL.md`, `NNNN-short-slug.md`, `@AGENTS.md`) that would each have needed an allowlist entry; a bare filename with no directory is not a path reference, and the alternative was four entries whose only content is that fact.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: `prose-duplication` skips any pair of files at the same relative path across the two halves — `template/X` against `X` — and `ARCHITECTURE.md` gains one clause stating that a live card sitting at a payload card's relative path is that card's instantiation.
  Rationale: the card's second legitimate class is "files a declared structural correspondence requires to be copies of each other", and the live half is by construction the filled form of the payload, so `AGENTS.md` and `template/AGENTS.md` already share long runs of working-rule text that the correspondence rule owns. Instantiating five cards adds five more such pairs, and `docs/decisions/0014-cards-are-project-content.md` removed cards from counterpart *existence* checking without saying what the relationship is when a counterpart does exist. Without the clause an executing session faces five enormous findings and invents an allowlist entry per card, which is an allowlist the size of the thing it excuses.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: `boundary-lint` binds to a machine-readable rule-id block inside `ARCHITECTURE.md`'s layer map section, and fails loudly when the set of ids there differs from the set it implements.
  Rationale: the card requires the check to fail rather than pass when it cannot find rules to apply, and prose is not parsable. The alternative considered was fingerprinting the section's text, which fails on every wording edit and teaches contributors to re-bless the fingerprint without reading. Ids in the same section as the prose that states them keep one owner for the rule and make both drift directions loud: an id in the map with no implementation, or an implementation with no id in the map.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: checks are POSIX `sh` with `awk`, `grep`, `sed`, `sort`, `find`, `cmp`, and `test`; no other dependency.
  Rationale: `GOALS.md` constrains every artifact to "markdown + git + shell", and this repository deliberately has no toolchain, so a check that needs one could not run. `awk` is part of the shell environment on every platform this repository is used on and is what makes the eight-word window comparison in M4 a single pass instead of several hundred process invocations.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: a card reaches `built` only in the milestone that observes its failing case, and reaches `enforced` in the same milestone once it is wired into `tools/verify`; the register in `docs/capabilities/index.md` remains the single home of status.
  Rationale: `docs/capabilities/CARD_FORMAT.md` sets the promotion bar and `docs/decisions/0007-card-status-single-home.md` puts status in one place. Splitting `built` and `enforced` across two milestones would leave a check that exists and runs but that nothing invokes, which is the state where a status is true and useless.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: the next free debt identifier is `D8`, and identifiers are not reused even after a row is deleted. This plan creates `D8` in M1, `D9` in M2, and `D10` in M6, and deletes the `D1` and `D3` rows.
  Rationale: `docs/DEBT.md` currently runs `D1` to `D7`, and rows are referenced from `AGENTS.md`, `ARCHITECTURE.md`, `docs/MATURITY.md`, `docs/capabilities/index.md`, and `docs/specs/bootstrap-flow.md`. Reusing a freed number would silently repoint an existing sentence at a different item.
  Date/Author: 2026-09-17, plan authoring session.

- Decision: `isolated-env` is not instantiated, and the reason moves into `GOALS.md` under Scope in M7 when the `D3` row that currently holds it is deleted.
  Rationale: `docs/DEBT.md` `D3` and `docs/decisions/0014-cards-are-project-content.md` both state that a card for an absent toolchain would be a check that passes on everything. Deleting the row that carries that reasoning without relocating one sentence of it would lose the only written answer to "why is there no `isolated-env` card here", and the next doc-garden pass would notice the payload ships six cards where the live half has five and treat it as an omission.
  Date/Author: 2026-09-17, plan authoring session.

## Outcomes & Retrospective

Not yet written: no milestone has been executed. At M7 this section compares the result against the purpose above on four points — whether one command decides all six invariants, what each check does not decide, whether the time budget published in `AGENTS.md` held, and which of the defects found during building were pre-existing rather than introduced by this work.

## Context and Orientation

### What this repository is

This repository is a document system, not a program. Nothing compiles, there is no test suite, and every artifact is markdown operated on with shell and git. It has two halves, and the distinction governs almost every edit made in it.

`template/` is the **payload**: the artifact set a target project receives by plain recursive copy and then fills in. Its files are skeletons carrying two kinds of authoring scaffolding — a **fill slot**, written as two opening braces, the word `FILL`, a colon, an instruction, and two closing braces; and a **guidance block**, an HTML comment whose first word is `GUIDANCE`. A payload file is fully filled when neither marker remains. The payload knows nothing about this repository: nothing under `template/` may name a path outside `template/`, and nothing there may mention this repository, the blueprint, or `skills/`.

The repository root is the **live instantiation** of that same payload for this project: `AGENTS.md`, `GOALS.md`, `ARCHITECTURE.md`, and the `docs/` tree are what the payload's skeletons look like once filled. This is what makes this repository client number one of its own bootstrap flow, and it is why divergence between the two halves is a bug in one of them.

Two further directories matter. `plans/` holds the plan convention in `plans/PLANS.md`, work in flight under `plans/active/`, and finished work under `plans/completed/`. `skills/` holds six portable procedures, one per directory, each a single `SKILL.md` an agent reads and follows.

Read `AGENTS.md` first in any session: it is the map that says which file owns which fact, and it is capped at about 100 lines on purpose.

### The vocabulary this plan uses

An **ExecPlan** is a document like this one: self-contained enough that a fresh session holding only it and a checkout can execute one milestone, record what it observed, and stop. The requirements are in `plans/PLANS.md`, which is this file's contract.

A **capability card** is a one-page specification for exactly one mechanical check: the invariant it decides, where it runs, the observable acceptance including a failing case written as an instruction, and the exact remediation text the check emits. Cards specify; they never contain the implementation. The format is `docs/capabilities/CARD_FORMAT.md`, and the five sections are Invariant, Enforcement point, Acceptance, Remediation message, and an optional Per-stack hints.

A card's **status** is one of `specced` (the card exists, nothing runs), `built` (the check exists, runs on demand, and has been *observed failing* on a real violation with its remediation text visible), and `enforced` (it runs somewhere it cannot be quietly skipped). Status lives in exactly one place, the register table in `docs/capabilities/index.md`, and never on a card.

The **maturity ladder** in `docs/MATURITY.md` has four rungs from L0 (human-gated, where this project stands) to L3. A rung may be claimed only when every card in that rung's row of the Gating capabilities table reads `enforced` and has been green for twenty consecutive landed changes. This plan does not claim a rung; it builds the machinery a later claim would rest on.

### What exists today

`docs/capabilities/` holds three cards this project wrote for itself — `template-live-drift`, `blueprint-eval`, `loop-runner` — all `specced`, plus `CARD_FORMAT.md` and the register `index.md`.

`template/docs/capabilities/` holds six generic cards the payload ships: `fast-verify`, `evidence-check`, `doc-integrity`, `boundary-lint`, `prose-duplication`, and `isolated-env`. Five of the six state invariants that hold in this repository too. `isolated-env` does not apply: there is no toolchain and no runtime here.

`AGENTS.md` publishes two commands under Commands. The first compares `template/plans/PLANS.md` with `plans/PLANS.md` byte for byte using `cmp`, where silence is a pass. The second greps a fixed set of live artifacts for the two authoring markers, where silence is also a pass; its pattern is written with the final letter of each marker word in brackets so that the command line does not match itself. That self-matching trick, and its limit — it cannot tell a real unfilled slot from a marker quoted in prose — is `D2` in `docs/DEBT.md`.

`docs/DEBT.md` runs `D1` through `D7`. `D1` is the hand-checked correspondence between the two halves and carries the hand procedure this plan replaces. `D3` is the uninstantiated generic cards and is marked overdue. `D6` (no brownfield ratchet for `boundary-lint`) and `D7` (the blueprint has never been run) are not this plan's work and stay.

### The invariants this plan mechanizes, and who owns each one

Each invariant is owned by exactly one file, and the check this plan builds decides it without restating why it matters.

`fast-verify` — one command, published in `AGENTS.md` with a written time budget, runs every check that is safe to run after any edit, exits nonzero if any of them fails, and never swallows the failing check's own remediation text.

`template-live-drift` — every file under `template/` has a live counterpart at the same relative path; `template/plans/PLANS.md` and `plans/PLANS.md` are byte-identical; for every other pair the template file's headings, minus those containing a fill slot, appear in the live file in the same order. Card files under `template/docs/capabilities/` and `.gitkeep` placeholders are excluded from counterpart existence. The comparison runs template to live only.

`doc-integrity` — every path reference in the live artifact half resolves to a file or directory that exists.

`prose-duplication` — no run of eight consecutive words appears in two of this project's markdown artifacts, outside the classes the card declares legitimate.

`evidence-check` — every plan file in `plans/active/` carries Progress, Surprises & Discoveries, Decision Log, and Outcomes & Retrospective, each non-empty, and every Progress entry marked complete carries a timestamp.

`boundary-lint` — no reference in this repository points in a direction the layer map in `ARCHITECTURE.md` forbids.

## Plan of Work

Seven milestones. Each one ends with the repository coherent: a command that works, a card whose status matches reality, and any prose that the change made false already corrected. Execute exactly one per session, record what you observed in the living sections above, and stop — `plans/PLANS.md` forbids continuing to the next milestone in the same session.

Every milestone follows the same shape, which is the procedure in `skills/capability-build/SKILL.md`: confirm the card is buildable, implement the decision rather than an approximation of it, make failure useful and success legible, run the passing case and confirm its counts are greater than zero, run the failing case exactly as the card instructs, wire the enforcement point and confirm it fires there, update the register row, and write what you observed into this plan. Where a check finds a violation in the existing tree, that is a finding to route — fix it or record it as debt — never a reason to narrow the card.

### M1 — One command to run after any edit

Acceptance, all four observable:

1. From the repository root, `tools/verify` exits zero, prints one line per check naming the check and what it examined with a count greater than zero, and ends with a line of the form `fast-verify: 2 of 2 checks passed (1s).`
2. Appending a line to `template/plans/PLANS.md` and re-running `tools/verify` produces a nonzero exit, and the output contains the failing check's own remediation text naming both `template/plans/PLANS.md` and `plans/PLANS.md` and the first differing line — not merely a summary. Reverting the line restores the passing run.
3. With `git config core.hooksPath tools/hooks` set, committing that same violation is refused by git, and the refusal output contains the same remediation text.
4. `AGENTS.md` under Commands names `tools/verify` as the cheap verification command with a time budget in seconds beside it, and no longer publishes the two hand commands; `docs/capabilities/index.md` shows `fast-verify` as `enforced` at `tools/verify`.

The work. Create `tools/verify`, `tools/checks/scaffolding-markers`, `tools/checks/plans-md-identity`, and `tools/hooks/pre-commit`, all `#!/bin/sh` and executable, following the protocol in Interfaces and Dependencies below. `scaffolding-markers` is the second published command moved into a file: it greps the eight skeleton-derived live artifacts for the two markers and reports how many files it scanned. `plans-md-identity` is the first published command moved into a file: `cmp` over the one byte-identical pair, with a remediation message naming both paths and the first differing line. Both are deliberately thin; M2 deletes `plans-md-identity` when `template-live-drift` subsumes it, and `scaffolding-markers` stays as an uncarded check because the marker definition it applies is owned by `template/AGENTS.md` and its known weakness is already tracked as `D2`.

Instantiate `docs/capabilities/fast-verify.md` by copying `template/docs/capabilities/fast-verify.md` and editing three things: the enforcement point becomes `tools/verify` and the pre-commit hook rather than continuous integration; the budget sentence names the number you measured and published; and the remediation example uses this repository's real check names.

Then repair the prose this milestone makes false, which is the part most easily missed. `AGENTS.md`'s Commands section is rewritten to publish `tools/verify`, its budget, and the one-time `git config core.hooksPath tools/hooks` install line; `D2`'s "Where" cell in `docs/DEBT.md` moves from the Commands section to `tools/checks/scaffolding-markers`, and its Details paragraph is updated to say where the bracketed-letter pattern now lives. `docs/capabilities/index.md`'s header paragraph currently says "Nothing is built yet: this project is at L0" and that the payload's starter set is not instantiated here; both claims start going stale in this milestone, so delete them and let the table's Status column answer — the rung is `docs/MATURITY.md`'s to state. `docs/MATURITY.md`'s Current rung section says nothing mechanical exists in this repository: rewrite it to say what now runs, that the rung remains L0, and that an L1 claim additionally needs the gate set complete and twenty green landed changes. Its closing paragraph under Gating capabilities, which lists the generic gates as "not instantiated here", is replaced by the single sentence that rows name only cards in this project's own register, and the L1 row for `fast-verify` is added.

Finally add debt row `D8`: the gate is skippable — `git commit --no-verify` bypasses the hook, and a fresh clone has no hook at all until the published config line is run, so no rung may be claimed on it. Where: `tools/hooks/pre-commit` against a clone that has not run the install line. Trigger: the first remote or continuous-integration system this repository gets.

### M2 — The correspondence walk becomes a command, and D1 is deleted

Acceptance, five observable:

1. `tools/checks/template-live-drift` on a clean tree exits zero and prints one line per pair naming which comparison it applied, plus a total line accounting for all 19 files under `template/`. The expected tally, to be confirmed rather than assumed: 1 byte-identical pair, 10 structural pairs, 8 exclusions — 6 card files and 2 `.gitkeep` placeholders.
2. Deleting the `## Register` heading from `docs/capabilities/index.md` and running `tools/verify` gives a nonzero exit naming that pair and the missing heading. Restore.
3. Appending a line to `template/plans/PLANS.md` gives a nonzero exit naming both paths and the first differing line. Restore.
4. Creating `template/docs/EXAMPLE.md` gives a nonzero exit naming the missing live counterpart `docs/EXAMPLE.md`. Delete the file.
5. `docs/DEBT.md` contains no `D1` row and no `D1` section; `grep -n 'D1' docs/DEBT.md AGENTS.md ARCHITECTURE.md docs/MATURITY.md` returns no line that still claims the walk is unmechanized.

The work. Write `tools/checks/template-live-drift` implementing the three comparisons the card specifies. The structural comparison is a subsequence walk: take the template file's headings in order, drop any heading whose text contains a fill slot, and require the remainder to appear in the live file in the same order — extra live headings are expected, because filled content adds them. Delete `tools/checks/plans-md-identity` and its entry in `tools/verify`'s check list, since byte identity is now one of the three comparisons; leaving both would be two mechanisms for one invariant.

Then retire the debt. Delete `D1`'s register row and its Details section from `docs/DEBT.md`. The two questions the check still cannot answer do not disappear with it: whether a live file's content is actually about the same subject as its template counterpart, and whether a live-only file ought to have had a template counterpart at all. Move both into the Invariant section of `docs/capabilities/template-live-drift.md` as an explicit statement of what the card does not decide — the same shape `template/docs/capabilities/doc-integrity.md` already uses for its two uncovered concerns — and add debt row `D9` recording that they remain a reading job, with the doc-garden pass named as where that reading happens. Replace the bullet under `ARCHITECTURE.md`'s Known rough edges, which currently says correspondence is maintained by hand, with one pointing at `D9`; the heading stays, because the payload's `## Known rough edges` heading carries no fill slot and structural correspondence requires it. Delete the paragraph in `AGENTS.md` that says the wider structural walk is hand-run and points at `D1`. Update the register row and confirm `docs/MATURITY.md`'s existing L1 row for this card now names an `enforced` check.

### M3 — Every path reference in the live half resolves

Acceptance, four observable:

1. `tools/checks/doc-integrity` on a clean tree exits zero and prints one line stating how many references it resolved — expect a number above one hundred — and how many allowlist entries it applied, which is 3.
2. Adding one map line to `AGENTS.md` that cites a backticked `docs/NOPE.md` — a hyphen, a space, the backticked path, an em dash, and the word placeholder — and running `tools/verify` gives a nonzero exit whose message names `AGENTS.md`, the line number, `docs/NOPE.md`, and the three possible next actions. Deleting the line restores the pass.
3. `docs/capabilities/index.md` shows `doc-integrity` as `enforced` at `tools/verify`, and `docs/MATURITY.md`'s Gating capabilities table carries an L2 row for it.
4. `template/docs/capabilities/doc-integrity.md` and `docs/capabilities/doc-integrity.md` both state that plan files are outside the checked set.

The work. Instantiate the card, then build the check. A reference is a backticked span or markdown link target that contains a slash and no space, no `*`, no angle bracket, and no brace — the exclusions are what keep the check from reporting commands (`` `cmp template/plans/PLANS.md plans/PLANS.md` ``), globs (`` `skills/*/SKILL.md` ``), and placeholder-bearing paths (`` `skills/<name>/SKILL.md` ``) as broken links. A reference resolves if it exists relative to the repository root or relative to the citing file's directory; the second case is the card's legitimate sibling class. Files whose own name ends in `_FORMAT.md` are skipped entirely, which is the card's illustrative-filename class.

The checked set is the live artifact half: `AGENTS.md`, `GOALS.md`, `ARCHITECTURE.md`, `CLAUDE.md`, everything under `docs/`, `plans/PLANS.md`, and everything under `skills/`. Two exclusions, each with its reason written into the card. Plan files under `plans/active/` and `plans/completed/` are out, because a plan names files before they exist and after they are deleted — the completed plan in this tree contains 23 such references. The payload half is out, because a reference inside `template/` must resolve inside the copy rather than from this repository's root, which is a different question owned by `boundary-lint`'s payload rule in M6; say so in the card so the gap reads as routed rather than forgotten.

Seed `tools/allow/doc-integrity.txt` with the three deliberate mentions this tree already contains, each with a one-line reason: `docs/capabilities/blueprint-eval.md` naming `skills/harness-init/README.md`, which is the file its failing case tells you to create by renaming; and `docs/capabilities/template-live-drift.md` naming `docs/EXAMPLE.md` and `template/docs/EXAMPLE.md`, which are the temporary files its third failing case creates. Anything else the first run reports is a genuine defect: fix it and record it in Surprises with the evidence.

### M4 — No fact is stated in two artifacts

Acceptance, four observable:

1. `tools/checks/prose-duplication` on a clean tree exits zero and prints one line stating how many artifacts it compared, how many eight-word windows it examined, and how many allowlist entries it applied — all three counts greater than zero.
2. Copying one whole sentence out of `docs/PRINCIPLES.md` into `AGENTS.md` and running `tools/verify` gives a nonzero exit naming both files, the shared window, and the three possible next actions. Deleting the copied sentence restores the pass.
3. `ARCHITECTURE.md`'s cross-cutting invariants state that a live card at a payload card's relative path is that card's instantiation, so the pair is a copy by construction.
4. `docs/capabilities/index.md` shows `prose-duplication` as `enforced` at `tools/verify` and `docs/MATURITY.md` gains no row for it — the card states that it removes the searching, not the routing, so it gates no rung.

The work. Normalize each artifact to its sequence of lowercase alphanumeric words after stripping fenced and indented blocks, take every window of eight consecutive words, and report any window shared by two artifacts. Eight is the floor the card sets and not a preference: shorter windows fire on ordinary English, longer ones miss a restated one-sentence rule. A single `awk` pass that maps each window to the files containing it decides every pair at once; the pairwise-intersection shape would be several hundred process invocations for the same answer.

The checked set is both halves — the live artifact half as in M3, plus everything under `template/` — minus records under `docs/decisions/`, minus plan files, and minus `tools/` since it holds no markdown. Five exclusions are decidable and one is a judgement. Decidable: a pair at the same relative path across the two halves, which `ARCHITECTURE.md`'s correspondence rule owns and M4 is where that clause is added; a pair whose files are governed by the same sibling `*_FORMAT.md` document, whose shared text is the format's; a pair of format documents themselves; a window in a section that also contains a backticked path to the file owning the fact, which is the attributed restatement `docs/PRINCIPLES.md` requires; and quoted material, already stripped before windowing. The judgement class — a proper-noun run, a path list, or a section-name list both files must spell out — is the allowlist at `tools/allow/prose-duplication.txt`.

Seed that allowlist from `docs/decisions/0016-skill-and-card-may-restate-one-fact.md`, which measured the exposure: `skills/doc-garden/SKILL.md` against `template/docs/capabilities/doc-integrity.md`, and `skills/harness-init/SKILL.md` against `template/docs/capabilities/isolated-env.md`, eleven shared windows between them. Both facts are load-bearing in both places and the two files are mutually unnameable, which is why that record exists. Note the consequence the record could not foresee: now that `doc-integrity` has a live instantiation, `skills/doc-garden/SKILL.md` pairs with `docs/capabilities/doc-integrity.md` as well, so each seeded entry needs a twin naming the live card. Make that rule explicit in the card: when a payload card is instantiated, every allowlist entry naming it gains a twin naming the live copy.

Sizing guard, because this is the milestone most likely to overflow: the v1 close found twelve pairs by hand and fixed eight, so the first run may report pairs nobody has seen. Fix the ones whose owner is obvious, and if more than three unexplained pairs remain, stop there — split this milestone in `Progress` as `completed: X; remaining: Y`, record the remaining pairs as a debt row with the evidence, and exit. Working through compaction is prohibited by `plans/PLANS.md`, and a duplication finding routed badly is a deleted sentence someone needed.

### M5 — Active plans carry their evidence

Acceptance, four observable:

1. `tools/checks/evidence-check` on a clean tree exits zero and prints one line per active plan naming the four sections it found — with this plan in flight there is exactly one such line.
2. Deleting the `## Decision Log` heading from this plan file and running `tools/verify` gives a nonzero exit naming the plan file and the missing section. Restore.
3. Marking any Progress entry in this plan complete without a timestamp and running `tools/verify` gives a nonzero exit naming the plan file and that entry. Restore.
4. `docs/capabilities/index.md` shows `evidence-check` as `enforced` at `tools/verify`, and `docs/MATURITY.md`'s Gating capabilities table carries an L1 row for it.

The work. Both halves of the failing case must be demonstrated before the card is `built`, because they are separate code paths and a checker that only counts headings passes the timestamp case blind. Presence of the four headings, non-emptiness of the text between them, and a timestamp on each completed checklist entry is the entire contract — do not parse the plan. An empty `plans/active/` is a passing state that says so and exits zero.

What the card deliberately does not decide, and which the instantiated copy must keep saying: whether the recorded evidence is true. "Never report a planned command as passing evidence" is a working rule in `AGENTS.md` enforced by review, and a check claiming to verify it would pass on everything.

Note for whoever executes this milestone: the failing cases mutate this plan file, which is also the file you are recording evidence into. Make the edit, run the check, copy the output into Surprises, then restore the file before committing — and confirm the restore by re-running `tools/verify`.

### M6 — The layer map's bans are decided by a command

Acceptance, five observable:

1. `tools/checks/boundary-lint` on a clean tree exits zero and prints how many references it examined and how many rules it applied; the rule count is 3 and the reference count is greater than zero.
2. Adding a reference to `docs/PRINCIPLES.md` inside any file under `template/` and running `tools/verify` gives a nonzero exit naming the offending file, line, target, and the rule id violated. Revert.
3. Renaming `ARCHITECTURE.md` temporarily and running the check gives a nonzero exit saying the layer map could not be read, rather than a pass over zero rules. Restore the name.
4. `ARCHITECTURE.md`'s layer map section carries a rule-id block whose ids are exactly the ones the check implements, and its allowed-target list for `skills/` includes `docs/capabilities/index.md` and `docs/specs/index.md`.
5. `docs/capabilities/index.md` shows `boundary-lint` as `enforced` at `tools/verify`, and `docs/MATURITY.md`'s Gating capabilities table carries an L2 row for it.

The work. In a repository with no compiled code an edge is a path reference from one component's files into another's, derived from file content and never from a hand-maintained list. Three of the layer map's four rules are decidable and get ids:

`no-outward-payload-reference` — for every file under `template/`, each path reference must resolve inside `template/`, either as `template/<reference>` or as a file whose parent directory resolves inside the payload. The parent-directory allowance is what makes an illustrative filename legitimate: `template/docs/capabilities/evidence-check.md` names `plans/active/add-search.md` inside a remediation transcript, and `template/plans/active/` exists in the payload while that file does not and should not. The same rule bans the words that name this repository, the blueprint, or `skills/` anywhere under `template/`.

`no-unprovided-skill-target` — for every file under `skills/`, each path reference must name an artifact the payload provides. That list is in `ARCHITECTURE.md` and, as authoring measured, it is currently wrong: the procedures name `docs/capabilities/index.md` seven times and `docs/specs/index.md` once, both of which the payload ships. Correct the list in `ARCHITECTURE.md` in this milestone and say why in this plan's Decision Log; the references are right and the map is what drifted. The clause banning `template/` in skill bodies belongs to this rule too.

`no-plan-file-dependency` — no file outside `plans/` may reference a plan file that exists. A reference to a plan path that resolves nowhere, such as the illustrative `plans/active/add-search.md`, is not a dependency, because a dependency needs a real target.

The fourth clause — skill bodies may not name a harness-specific tool — is not decidable without a list of every harness tool name, so it stays out of the check. State that in the instantiated card as a limitation with review named as its owner, add debt row `D10` for it, and add a second bullet under `ARCHITECTURE.md`'s Known rough edges pointing at that row. `capability-build`'s rule applies here: cover exactly the part that is decidable and keep the rest out rather than half-deciding the invariant.

The rule-id block goes inside the `## Layer map and dependency rules` section of `ARCHITECTURE.md` as an indented block, one line per rule, each line carrying the id and a one-clause restatement of the scope and the ban. The check parses that block and refuses to run — exit 2, "cannot run" — if the section is missing, the block is absent or empty, or the set of ids there differs from the set it implements in either direction. Both drift directions must be loud: an id in the map with no implementation is a rule nobody enforces, and an implementation with no id is a rule nobody wrote down. The live `boundary-lint` card records this binding, since a builder reading the card alone would otherwise invent a second mechanism.

Note that `D6` in `docs/DEBT.md` — no retrofit path for a brownfield codebase — is untouched by this milestone. It concerns the payload's card on a target project whose layer map is already violated. This repository's map is not violated except by the map's own stale list, which this milestone fixes.

### M7 — Close the plan out

Acceptance, five observable:

1. `tools/verify` exits zero, names six checks, and its reported wall-clock time is inside the budget published in `AGENTS.md`; if it is not, the budget line and the check set are adjusted in this same milestone until the two agree.
2. `docs/capabilities/index.md` lists eight cards; the six this plan built read `enforced` at `tools/verify`, and `blueprint-eval` and `loop-runner` still read `specced`.
3. `docs/DEBT.md` contains no `D3` row and no `D3` section, and `GOALS.md` under Scope states in one sentence why `isolated-env` is not instantiated here.
4. `docs/specs/` carries a file describing what a reader can now run, what each check decides, and what each one deliberately leaves to a human, listed in that directory's index table.
5. This plan's `Outcomes & Retrospective` is written against the four points it names above, and the file has moved to `plans/completed/`.

The work is the lifecycle `plans/PLANS.md` requires plus the two deletions this plan owes. Reflect the behavior into `docs/specs/` — a completed plan is an archive of a change, not a living description of current behavior, so the spec file is where "what this repository checks" lives afterwards. Graduate the authoring and building decisions that still bind future work into `docs/decisions/`, numbered from `0017` upward, each in the four-field format `docs/decisions/DECISION_FORMAT.md` sets; the enforcement-point choice, the same-relative-path duplication exclusion, and the rule-id binding are the three strongest candidates, and a decision that only mattered for one milestone stays in this plan's log. Delete `D3`'s row and section, moving the `isolated-env` sentence into `GOALS.md` first. Then write the retrospective and move this file.

## Concrete Steps

All commands run from the repository root; each script and each command below assumes it, and `cd "$(git rev-parse --show-toplevel)"` is how a script gets there rather than hard-coding a path. This section is updated as work proceeds; the transcripts below are **expected**, written at authoring time, and no command has been run against an implementation that does not yet exist.

M1, in order:

    mkdir -p tools/checks tools/allow tools/hooks
    # author tools/checks/scaffolding-markers, tools/checks/plans-md-identity,
    # tools/verify, tools/hooks/pre-commit
    chmod +x tools/verify tools/checks/* tools/hooks/pre-commit
    time ./tools/verify

Expected output on a clean tree:

    plans-md-identity: ok — 1 pair compared byte for byte.
    scaffolding-markers: ok — 8 artifacts scanned, 0 markers found.
    fast-verify: 2 of 2 checks passed (1s).

The failing case, then the revert:

    printf 'drift\n' >> template/plans/PLANS.md
    ./tools/verify; echo "exit=$?"

Expected:

    plans-md-identity: template/plans/PLANS.md and plans/PLANS.md differ,
    first at line 153. These two files are byte-identical by construction.
    Copy the intended version over the other and re-run.
    scaffolding-markers: ok — 8 artifacts scanned, 0 markers found.
    fast-verify: 1 of 2 checks failed.
    Fix the violations reported above and re-run ./tools/verify.
    exit=1

    git checkout -- template/plans/PLANS.md
    ./tools/verify; echo "exit=$?"

The hook, demonstrated rather than assumed:

    git config core.hooksPath tools/hooks
    printf 'drift\n' >> template/plans/PLANS.md
    git add template/plans/PLANS.md && git commit -m "should be refused"

Expect the same remediation text and no new commit; confirm with `git log --oneline -1`, then `git reset HEAD template/plans/PLANS.md && git checkout -- template/plans/PLANS.md`.

M2 through M6 each follow the same three steps: write `tools/checks/<card-name>`, add it to the check list in `tools/verify`, then run the card's passing case and every failing case it specifies, copying the real output into `Surprises & Discoveries`. The failing cases are quoted in each milestone's Acceptance above; run them exactly as written, and if a check does not fail on a violation, the check is wrong — do not adjust the card to match what was built.

M7:

    time ./tools/verify
    grep -n 'D3' docs/DEBT.md ; grep -c '^| ' docs/capabilities/index.md

Expect no `D3` line and a row count matching eight cards plus the table's two header rows.

## Validation and Acceptance

There is no build and no test suite; `tools/verify` is the whole validation story, and each milestone's acceptance above is phrased as commands and the output they must produce.

The bar for every check, taken from `docs/capabilities/CARD_FORMAT.md` and `skills/capability-build/SKILL.md`: a passing run prints a count greater than zero, because an empty scope — a wrong path, a glob matching nothing, a filter excluding everything — is the most common silent failure and passes forever; and a failing run exits nonzero with the card's remediation text carrying the real offender, because the failure message is the only documentation read at the moment someone is wrong.

A card may be marked `built` only after its failing case has been observed with that text visible, and `enforced` only once it runs from `tools/verify`. Record the violating change, the exact command, and the exact failure text in this plan — never on the card, which stays a specification.

Two repository-wide checks close each session, replacing the pair that `AGENTS.md` publishes today once M1 lands: run `./tools/verify` and record its output, and read the diff for prose that the change made false. The second one is not mechanizable and is why `docs/DEBT.md` `D9` exists.

## Idempotence and Recovery

Every step is repeatable. The checks read the tree and never write to it, so `tools/verify` may be run any number of times, and re-running a milestone's failing case after a restore is how you confirm the restore.

Each failing case mutates the tree deliberately. Restore with `git checkout -- <path>` for an edited file, `rm` for a created one, and `git mv` back for a rename, then re-run `tools/verify` and confirm the pass returns. Never leave a violating change in the tree after the demonstration, and never commit one: if a commit is refused by the hook, the fix is to revert the violation, not to pass `--no-verify`.

If a milestone will not fit in the session — context filling, compaction approaching — split it in place in `Progress` as `completed: X; remaining: Y`, record the split in the `Decision Log`, and exit. A compacted session silently breaks the requirement that the work be restartable from this file alone.

Recovery from a half-built check is to delete the script and its line in `tools/verify`'s check list, leaving the register row at its previous status. A card whose row says `enforced` while `tools/verify` does not run it is the one state this plan must never leave behind, because `docs/MATURITY.md`'s promotion rule reads that column as fact.

## Artifacts and Notes

The authoring measurements behind the contracts above, all taken on 2026-09-17 against this tree, so a later session can re-derive them rather than re-discovering them:

    19 files under template/ — 1 byte-identical pair (plans/PLANS.md),
    10 structural pairs, 8 exclusions (6 card files, 2 .gitkeep).
    55 markdown files in the repository; 34,578 words outside
    plans/completed/, of which the completed plan adds a further 15,395.
    plans/completed/v1-blueprint.md: 54 distinct .md references,
    23 of which resolve to nothing.
    Live half, slash-bearing backticked references that resolve nowhere:
      docs/capabilities/blueprint-eval.md -> skills/harness-init/README.md
      docs/capabilities/template-live-drift.md -> docs/EXAMPLE.md
      docs/capabilities/template-live-drift.md -> template/docs/EXAMPLE.md
    All three are deliberate mentions inside failing-case instructions.
    skills/ references to payload artifacts absent from the layer map's
    allowed-target list: docs/capabilities/index.md (7), docs/specs/index.md (1).
    template/ mentions of this repository, the blueprint, or skills/: none.
    References to a plan file from outside plans/: none that resolve.
    git remote: none. .github: absent. core.hooksPath: unset.

## Interfaces and Dependencies

These contracts are what a later session cannot rediscover, so they are fixed here. Any change to one is a logged decision, not a preference.

**Layout.** `tools/verify` is the aggregator and the command `AGENTS.md` publishes. `tools/checks/<check-name>` is one executable per check, named exactly as its card, plus the uncarded `scaffolding-markers`. `tools/allow/<check-name>.txt` holds allowlists. `tools/hooks/pre-commit` is the versioned hook. Nothing under `tools/` is copied into `template/`.

**Language.** POSIX `sh` with `awk`, `grep`, `sed`, `sort`, `find`, `cmp`, and `test`. No other dependency, no bashisms, no network. Every script starts with `#!/bin/sh`, runs with no arguments, and begins by changing to the repository root with `cd "$(git rev-parse --show-toplevel)" || exit 2` so that it behaves identically from a subdirectory and from a git hook.

**Check protocol.** Exit 0 means the invariant holds, and the check prints exactly one line: `<check-name>: ok — <what was examined, with counts>`. Exit 1 means a violation, and the check prints one block per violation built from its card's remediation text with the real offender substituted — file, line, target, rule — then the count. Exit 2 means the check could not decide, and it prints `<check-name>: cannot run — <reason>` with the next action; `tools/verify` treats 2 as a failure and never as a pass. No check writes to the tree.

**Aggregator protocol.** `tools/verify` runs the checks in a fixed order written in the script — a literal list, never a glob, so that output order is stable — and streams each child's output unmodified, because a summary that swallows a remediation message fails `fast-verify`'s own acceptance. It ends with `fast-verify: <M> of <M> checks passed (<seconds>s).` and exit 0, or one line per failure plus `fast-verify: <N> of <M> checks failed.` and `Fix the violations reported above and re-run ./tools/verify.` and exit 1. The check list at the end of this plan's work is: `scaffolding-markers`, `template-live-drift`, `doc-integrity`, `prose-duplication`, `evidence-check`, `boundary-lint`.

**Allowlist format.** One entry per line: a key, two spaces, `#`, and a one-line reason. Blank lines and lines beginning with `#` are ignored. Keys are `<citing-path>:<reference>` for `doc-integrity` and `<fileA>:<fileB>:<window>` for `prose-duplication`, matching the remediation text each card publishes. Entries that match nothing are reported as a stale count in the passing summary line so they decay visibly; they do not fail the check, because the card states one invariant and this is not it.

**Time budget.** `AGENTS.md` publishes a budget in seconds beside the command, because an agent that cannot predict what a command costs stops running it. Set it in M1 to twice the observed clean run, rounded up to five seconds, and never above sixty. Each later milestone re-measures; if the number no longer holds, the budget line and the check set are corrected in that same commit.

**Identifiers already spent.** Debt rows `D1` through `D7` exist; this plan creates `D8` (M1), `D9` (M2), `D10` (M6) and deletes the `D1` and `D3` rows. Decision records run `0001` through `0016`; graduation in M7 starts at `0017`. Neither series reuses a number.

**Files this plan edits outside `tools/`.** `AGENTS.md` (Commands, and the hand-walk paragraph), `ARCHITECTURE.md` (a component entry for `tools/`, the rule-id block, the allowed-target list, the instantiated-card clause, Known rough edges), `GOALS.md` (Scope, one sentence in M7), `docs/DEBT.md` (`D2`'s location, `D8`/`D9`/`D10` added, `D1`/`D3` deleted), `docs/MATURITY.md` (Current rung, four gating rows, the closing paragraph), `docs/capabilities/index.md` (header prose and six rows), `docs/capabilities/template-live-drift.md` (what it does not decide), five new card files under `docs/capabilities/`, `template/docs/capabilities/doc-integrity.md` (the plan-file exclusion, the one edit this plan makes to the payload), `docs/specs/` (a new spec and its index row), and `docs/decisions/` (new records in M7).

**What this plan deliberately does not do.** It does not instantiate `isolated-env`, which would be a check that passes on everything in a repository with no toolchain. It does not build `blueprint-eval` or `loop-runner`, which need a live trial and an unattended loop that `docs/DEBT.md` `D5` and `D7` park. It does not claim L1 on the ladder: the promotion rule needs twenty consecutive green landed changes and a named human, and this plan produces neither. It does not add continuous integration, because there is no remote to add it to. It does not touch `D4` (skill auto-discovery) or `D6` (the brownfield ratchet for `boundary-lint`), both of which are payload questions that a real target project answers.
