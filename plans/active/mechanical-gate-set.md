# Build this repository's mechanical gate set: instantiate the payload's generic capability cards and make every one of them a running check

This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

This document must be maintained in accordance with `plans/PLANS.md`.

## Purpose / Big Picture

Today nothing in this repository is checked by a machine. Two commands are published under Commands in `AGENTS.md` and a person runs them when they remember; every other invariant this project claims — references resolve, no fact is stated twice, active plans carry their evidence sections, the layer map's dependency bans hold, the payload half and the live half correspond — is enforced by someone reading carefully. `docs/DEBT.md` records the consequence: at the close of v1 a hand sweep found a decision record citing a path that resolved from nowhere, and twelve pairs of artifacts sharing prose, eight of which were real single-owner violations. Both classes of defect had been in the tree for at least one milestone before anyone looked.

After this plan, a reader who has just cloned this repository can type one command, `tools/verify`, and watch six named checks decide those invariants in a few seconds, each one printing what it examined and how much of it. Introducing any of the violations the capability cards describe — a map line pointing at a file that does not exist, a sentence copied out of `docs/PRINCIPLES.md` into `AGENTS.md`, a heading deleted from `docs/capabilities/index.md`, a completed Progress entry with no timestamp, a path reference inside `template/` pointing outside it — makes that command exit nonzero and print the offender's file, line, and the next action to take. Attempting to commit such a violation with the repository's hook installed makes git refuse the commit.

Two debt items are paid by that outcome. `docs/DEBT.md` `D3` — the payload's generic capability cards are not instantiated for this project — is paid by instantiating the five applicable cards in `docs/capabilities/` and building each one. `docs/DEBT.md` `D1` — template ↔ live correspondence is checked by hand — is paid by building the check that `docs/capabilities/template-live-drift.md` already specifies, after which the hand walk recorded in that row is deleted rather than rewritten.

What an outside reader sees, concretely: `tools/verify` exists and passes; `docs/capabilities/index.md` lists eight cards of which six read `enforced` with `tools/verify` in the "Enforced at" column; `docs/DEBT.md` no longer contains `D1` or `D3`; and `docs/specs/` describes what each check decides and what it deliberately leaves to a human.

## Progress

- [x] (2026-09-17 15:23Z) M1 — `tools/verify` exists, aggregates the two checks this repository ran by hand, and is published in `AGENTS.md` with a 5-second budget; `fast-verify` instantiated and `enforced`; pre-commit hook wired and observed refusing a commit.
- [x] (2026-09-17 15:37Z) M2 — `template-live-drift` built and `enforced`; all three of its comparisons demonstrated failing through `./tools/verify` and restored; `plans-md-identity` deleted; `D1` deleted from `docs/DEBT.md` and `D9` added.
- [x] (2026-09-17 15:55Z) M3 — `doc-integrity` instantiated, built and `enforced`; allowlist seeded with the three deliberate mentions and all three observed applying; the failing case demonstrated through `./tools/verify` and through the hook, then restored; a fourth decidable class — quoted material in fenced and indented blocks — added to both halves of the card.
- [x] (2026-09-17 17:30Z) M4 — `prose-duplication` instantiated, built and `enforced`; HTML comment blocks added to the quoted-material class in both halves; the first run reported 8 pairs — 3 repaired in the non-owning file, 1 a defect in the check itself, 4 allowlisted as the judgement class over 20 window keys; the failing case demonstrated through `./tools/verify` and through the hook, then restored.
- [x] (2026-09-17 17:48Z) M5 — `evidence-check` instantiated, built and `enforced`; both failing halves demonstrated against this plan file through `./tools/verify` and the second through the hook, then restored; the check's four other paths exercised too, and one defect found in `prose-duplication`'s attribution class and fixed there.
- [x] (2026-09-17 18:05Z) M6 — `boundary-lint` instantiated, built and `enforced`; the rule-id block added to `ARCHITECTURE.md` and the check bound to it; the allowed-target list corrected in three places, not the one authoring measured; all five violation shapes and all four binding failures observed; the outward-reference case demonstrated through `./tools/verify` and through the hook, then restored; `D10` added.
- [ ] M7 — close-out: budget re-measured and published, behavior reflected into `docs/specs/`, authoring decisions graduated to `docs/decisions/`, `D3` deleted, plan moved to `plans/completed/`.

Use timestamps to measure rates of progress. M1 through M6 were each executed in one session on 2026-09-17; M7 has not been started.

## Surprises & Discoveries

Three things were measured while authoring, and each one changed a milestone's contract. They are recorded here because they were observed in the tree, not reasoned about.

- Observation: the completed plan `plans/completed/v1-blueprint.md` names 23 distinct markdown paths that do not exist in the tree, out of 54 distinct path-shaped references it contains.
  Evidence: extracting backticked spans ending in `.md` from that file and testing each for existence yielded 23 misses. A completed plan legitimately names files it created and later deleted, files it renamed, and illustrative paths from remediation transcripts, so a reference checker pointed at `plans/completed/` reports 23 findings and zero defects. This is why M3 fixes the checked set to the live artifact half and excludes plan files in both directories.

- Observation: the procedures under `skills/` name `docs/capabilities/index.md` seven times and `docs/specs/index.md` once, and neither path appears in the allowed-target list that `ARCHITECTURE.md` writes into its layer map.
  Evidence: extracting backticked path spans from `skills/*/SKILL.md` and comparing against the list in `ARCHITECTURE.md` under "Layer map and dependency rules" — the list names the three `docs/` directories "and their format documents", and an index file is neither a directory nor a format document. The payload does ship both index files (`template/docs/capabilities/index.md`, `template/docs/specs/index.md`), so the references are legitimate and the list is what is wrong. M6 corrects the list rather than editing eight references.

- Observation: no git remote is configured, there is no `.github` directory, and `core.hooksPath` is unset.
  Evidence: `git remote -v` prints nothing; the repository root contains only `.git`, the four root markdown files, and the `docs`, `plans`, `skills`, `template` directories. Every card in `template/docs/capabilities/` names continuous integration as half of its enforcement point, and there is no continuous integration here to name. M1 settles what the enforcement point is instead, and records the resulting weakness as debt rather than claiming a gate that does not exist.

- Observation (M1): `cmp` reports no line number when one file is a prefix of the other, which is exactly what the card's own failing case produces.
  Evidence: after `printf 'drift\n' >> template/plans/PLANS.md`, `cmp` printed `cmp: EOF on plans/PLANS.md` and nothing else, so the first version of `tools/checks/plans-md-identity` — which parsed the line number out of `cmp`'s message — printed "one file is a prefix of the other" where M1's acceptance requires the first differing line. The check now decides byte identity with `cmp` and locates the divergence with a two-file `awk` pass that also covers the shorter-live and mid-file cases. Observed after the fix, on three shapes: append to `template/plans/PLANS.md` → `first at line 175`; truncate `plans/PLANS.md` to 100 lines → `first at line 101`; mutate line 30 of the template copy → `first at line 30`.

- Observation (M1): the hook judges the working tree, not the staged content.
  Evidence: `tools/hooks/pre-commit` invokes `./tools/verify`, which reads files from the checkout, so `git commit` of a clean subset of a dirty tree is refused on the strength of the unstaged violation. It errs toward refusing too much rather than letting a violation through, so it is recorded as one of three weaknesses in `docs/DEBT.md` `D8` rather than treated as a defect in M1.

- Observation (M1): the clean run costs half a second, an order of magnitude under the plan's floor.
  Evidence: `time ./tools/verify` reported `real 0m0.500s` on the first clean run and `0s` in the aggregator's own whole-second summary. The budget contract says twice the observed run rounded up to five seconds, so the published budget is 5 seconds, and `tools/verify`'s summary line reads `(0s)` — the aggregator counts whole seconds, which is honest at this size and will need tenths only if a later check makes the run slow enough for the budget to bind.

- Observation (M2): the authoring tally held exactly, and the heading arithmetic behind it is 54 template headings minus the 6 that carry a fill slot.
  Evidence: `./tools/checks/template-live-drift` on a clean tree ends with `ok — 19 files under template/: 1 byte-identical, 10 by heading subsequence over 48 headings, 8 excluded.` The six dropped headings are line 1 of `template/AGENTS.md`, lines 61 and 66 of `template/docs/PRINCIPLES.md`, line 65 of `template/docs/DEBT.md`, and lines 53 and 59 of `template/ARCHITECTURE.md` — every one a section whose title is itself filled in on arrival.

- Observation (M2): nothing under `template/` uses a fenced code block, so the fence guard in the heading extractor is a branch this tree never exercises.
  Evidence: `grep -rn '```' template/` printed nothing. The guard stays, because a skeleton that fences an example would otherwise have its example headings read as required ones, but it is untested here and the first skeleton to fence anything is the first real test of it.

- Observation (M2): deleting `tools/checks/plans-md-identity` did not delete what it knew.
  Evidence: M1's finding that `cmp` reports no line number when one file is a prefix of the other still holds, so the two-file `awk` pass that locates the divergence was carried into `tools/checks/template-live-drift` rather than dropped with the script. The append failing case printed `first at line 175`, the same location M1 observed.

- Observation (M2): acceptance clause 5's `grep -n 'D1' …` is a clean test today and will stop being one in M6.
  Evidence: run after the deletion it printed nothing and exited 1. The pattern has no word boundary, so once M6 creates `D10` the same command matches that row. A later session checking this should use `grep -n 'D1\b'` or read the register directly.

- Observation (M3): the authoring tally of three non-resolving references held exactly, and the tree gained no new one across M1 and M2.
  Evidence: the first run of `./tools/checks/doc-integrity`, before the card existed, printed `ok — 257 of 260 references resolved in 37 artifacts, 2 format documents skipped, 3 allowlist entries applied, 0 stale.` — three unresolved, all three matched by the seeded allowlist, none stale. The second milestone in a row to confirm an authoring measurement rather than correct it.

- Observation (M3): the card cannot state its own remediation message without the quoted-material class, and that class suppresses nothing else in this tree.
  Evidence: an inverted extractor — same reference rules, reading only fenced and indented blocks — found 5 block-quoted references in the live half, all of which resolve, so the class hid no finding before this milestone. After `docs/capabilities/doc-integrity.md` landed, the same inverted run printed `would report: docs/capabilities/doc-integrity.md:85 -> docs/NOPE.md` and `would report: docs/capabilities/doc-integrity.md:87 -> AGENTS.md:docs/NOPE.md` — the card's own remediation block and its own allowlist key. Without the class the check reports itself, and the only alternative is two allowlist entries whose reason is that the card is a card.

- Observation (M3): the live half contains no markdown link syntax at all, so the link-target arm of the extractor is a branch this tree never exercises.
  Evidence: `grep -rn '](' --include='*.md'` over the checked set printed nothing; all 277 references are backticked paths. The arm stays, because the payload's own card says a checker must accept both forms and a target project may well write links, but it is untested here — the same shape as M2's untested fence guard.

- Observation (M3): a check whose remediation line embeds an allowlist key produces a key that looks like a path and is not one.
  Evidence: the violation line names `` `AGENTS.md:docs/NOPE.md` `` as the thing to add, and that span contains a slash and no excluded character, so the extractor treats it as a reference. It is only harmless because remediation text lives in an indented block; a card that quoted its own key in prose would report a second, phantom violation on every run.

- Observation (M4): the first run found 8 pairs, none of them among the twelve the v1 close found by hand, and all 8 explainable — the sizing guard's "pairs nobody has seen" happened, and its stop condition was never reached.
  Evidence: `./tools/checks/prose-duplication` with an empty allowlist printed `prose-duplication: 8 duplicated pairs in 27769 windows across 41 artifacts.` Three were repaired in the non-owning file — the three cards whose Enforcement point sentence collided with the `tools/` inventory in `ARCHITECTURE.md`, two of them also restating in their Per-stack hints the markdown-shell-git constraint that same file owns. One was a defect in the check, below. Four went to the allowlist as the judgement class.

- Observation (M4): the same restatement read as attributed in the live half and unattributed in the payload, because a payload file that cites `AGENTS.md` is citing `template/AGENTS.md`.
  Evidence: the first run reported `template/AGENTS.md and template/docs/capabilities/evidence-check.md share the window "never report a planned command as passing evidence"` while saying nothing about `AGENTS.md` against that same card — the card's Invariant section cites `AGENTS.md`, which the attribution class resolved for the live pair and not for the payload pair. The card is right: the rule it quotes is owned by the `AGENTS.md` the target project receives. Fixed in the check rather than allowlisted, since an allowlist entry there would have excused a correctly attributed restatement.

- Observation (M4): an allowlist entry for the enforcement-point collision would have grown by one entry per card this plan still builds.
  Evidence: the window `and the versioned hook tools hooks pre commit` was shared by `ARCHITECTURE.md` with all three cards that existed — `doc-integrity`, `fast-verify`, `template-live-drift` — because every card's Enforcement point names the same hook path in the same phrasing, and M5 and M6 each add another. The `tools/` component entry in `ARCHITECTURE.md` was reworded instead; it is the non-owning file for a card's enforcement point, and an inventory has no reason to phrase itself like one.

- Observation (M4): the new card could not state its own same-relative-path class without `doc-integrity` reporting it, which is the second card in a row to fail a sibling check on its own specification text.
  Evidence: the clause first read "`template/X` against `X`", and the aggregator printed `doc-integrity: docs/capabilities/prose-duplication.md:36 references \`template/X\`, which does not exist.` — a backticked span with a slash and no excluded character is a reference no matter what it means. Rewritten as prose naming the two halves, after which `./tools/verify` passed. M3 hit the same shape from the other direction and recorded the same rule: a card that must write an unresolvable path writes it in an indented block or not in backticks at all.

- Observation (M4): the whole invariant costs 0.4 seconds over 29,129 windows, and the per-window allowlist is four times the size of the pair count it covers.
  Evidence: `time ./tools/verify` reported `real 0m0.397s` with all four checks passing, against the 5-second budget `AGENTS.md` publishes, so the budget line needs no change in this milestone. The passing summary reads `prose-duplication: ok — 42 artifacts compared, 29129 eight-word windows examined, 20 allowlist entries applied, 0 stale.` — 20 keys for 4 pairs, because the key format in Interfaces and Dependencies is one window per entry and a pair-level key would excuse duplication nobody has read yet.

- Observation (M5): the new card's own out-of-scope sentence collided with the payload's `AGENTS.md`, and the pair the check reported was the diagonal of the correspondence rather than the citation the card actually makes.
  Evidence: with the card written and attributing the working rule to `AGENTS.md`, `./tools/verify` printed `prose-duplication: docs/capabilities/evidence-check.md and template/AGENTS.md share the window "never report a planned command as passing evidence"` — and said nothing about the live `AGENTS.md`, which the attribution class resolved. The window exists in both copies of `AGENTS.md` because the live file is the filled form of the payload skeleton, so a card citing the live owner is unattributed against the payload copy of that same owner. This is M4's payload-citation observation seen from the other side: M4 fixed the citing end, and the target end had the same hole.

- Observation (M5): the attribution fix excused nothing that was already allowlisted.
  Evidence: after adding the fourth citation form, `prose-duplication: ok — 43 artifacts compared, 30011 eight-word windows examined, 20 allowlist entries applied, 0 stale.` — the same 20 keys applied and none decayed, so the new form widened the class by exactly the one pair it was written for rather than quietly covering the judgement class.

- Observation (M5): the plan file's Progress tallies are what the check reports, so the summary line moves as the plan is written.
  Evidence: the clean run printed `4 of 7 Progress entries complete, every one timestamped.` before this milestone's entry was ticked, against a Progress section holding four completed and three open entries — the check reads the tree, and its own count is a fact about the file it is run beside rather than a constant.

- Observation (M5): the remediation's timestamp example is a format, not a date, which is a deviation from the generic card.
  Evidence: the first implementation printed `- [x] (2026-09-17 15:23Z) M5 — …`, reusing M1's real observation time inside a message that asks the reader for a time they observed. `template/docs/capabilities/evidence-check.md` shows a concrete date in the same position. A message that asks for an observation and hands over a paste-ready date invites the fabrication the working rule in `AGENTS.md` forbids, so the shape `(YYYY-MM-DD HH:MMZ)` is printed instead; the failing case was re-run after the change and the card records the reason.

- Observation (M5): the four paths the milestone's acceptance does not name were exercised anyway, and all four behaved.
  Evidence: a throwaway second plan under `plans/active/` — one ticked entry with no stamp, an empty `## Surprises & Discoveries`, and no `## Outcomes & Retrospective` — produced all three message shapes at once, two per-plan lines, and `evidence-check: 3 violations in 2 active plans, 8 Progress entries read.`; and with this plan moved aside, `evidence-check: ok — no active plans under plans/active/, 4 sections required of each.` at exit 0. Both were restored and the clean run re-observed. The empty-section message and the empty-directory pass are published on the card, and a published message nobody has seen print is a message that has never been checked.

- Observation (M6): the layer map's allowed-target list was wrong in three places, not the one authoring measured, and the third was the procedure directory itself.
  Evidence: extracting every slash-bearing backticked reference from the six files under `skills/` gave 74, and every one of them is legitimate under the corrected list. Three are `skills/` as a bare directory — `skills/harness-init/SKILL.md` lines 51 and 121 and `skills/retro/SKILL.md` line 96, each naming the procedure set as a set — where the list named only `skills/<name>/SKILL.md`; eight more name `docs/capabilities/index.md` or `docs/specs/index.md`, the two the authoring measurement found. Authoring stopped at those two. The references are right in all three places and the list is what drifted, so it gained the component root beside them.

- Observation (M6): 36 references from outside `plans/` name `plans/PLANS.md`, so the plan-file rule had to say in the map what it had only implied.
  Evidence: the same extraction over the live half, the payload and the procedures found 36 references to that path from files that are not plans, and zero references to any file under `plans/active/` or `plans/completed/` that exists. The map's sentence read "nothing outside `plans/` may depend on a specific plan file" while its own allowed-target list named `plans/PLANS.md`, which is a contradiction only a reader resolves. The paragraph now says the convention document is not a plan and the rule's scope is the two work directories; without that the check would have reported 36 violations against a rule nobody meant.

- Observation (M6): the plan's own example for the parent-directory allowance is not a reference at all, and the allowance is load-bearing for a different file.
  Evidence: M6's work section justifies the allowance with `template/docs/capabilities/evidence-check.md` naming `plans/active/add-search.md` in a remediation transcript; `grep -rn 'add-search' template/` shows both mentions are bare text inside an indented block, not backticked, so no extractor sees them. What does need the allowance is the payload's `doc-integrity` card, which names a must-not-exist path in backticks twice in prose (lines 57 and 59) and once in its remediation block, with `template/docs/` shipped and the file deliberately absent. The allowance stands; the example behind it was wrong.

- Observation (M6): M3's phantom allowlist key bit a second check, and this time it could not be ignored.
  Evidence: the first extraction reported `template/docs/capabilities/doc-integrity.md:69 -> AGENTS.md:docs/NOPE.md` — the allowlist key the card tells a reader to add, which contains a slash and none of the excluded characters. M3 recorded the shape as harmless because remediation text lives in an indented block and that check skips such blocks; this check deliberately reads them, so the key arrived as a reference whose parent directory is a filename. The reference definition here excludes any span containing a colon: a file-and-line locator and a compound key both contain a slash, and neither is a path.

- Observation (M6): the check passed on its first run, so every violation path was probed deliberately rather than discovered.
  Evidence: `./tools/checks/boundary-lint` printed `ok — 418 references examined in 58 files, 3 rules applied from ARCHITECTURE.md, no banned mention in 17 payload files or 6 procedures.` before any demonstration — the third milestone in a row to confirm an authoring measurement rather than correct one, and the first whose check found nothing at all to route. Five single-line mutations then produced all five violation shapes, and four mutations of the map produced all four binding failures; both sets are transcribed in `Concrete Steps`.

- Observation (M6): a skill body naming the payload directory in a backticked path broke one rule and printed two blocks.
  Evidence: appending ``Copy from `template/AGENTS.md`.`` to `skills/plan-author/SKILL.md` reported the unprovided-target block and the banned-mention block against the same line, because the path scan and the word scan both see it. Both blocks name the same word and the same fix, so the mention scan now skips a line already reported for its reference, and the same mutation prints one block.

- Observation (M6): six checks cost half a second, the same as five.
  Evidence: `time ./tools/verify` reported `real 0m0.505s` with `fast-verify: 6 of 6 checks passed`, against the 5-second budget `AGENTS.md` publishes, so the budget line needs no change in this milestone either. The new check's own share is 0.15 to 0.28 seconds over 440 references, which is the largest single share so far and still an order of magnitude inside the budget.

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

- Decision: (pre-execution review) `prose-duplication`'s normalization strips HTML comment blocks alongside fenced and indented blocks, and the template card's quoted-material class is amended to say so before instantiation — making the payload edits two, not one.
  Rationale: the checked set includes `template/`, whose eight skeletons share a verbatim marker-definition guidance block by design — measured: `grep -l` for the shared sentence returns 8 files, which is twenty-eight pair findings covered by no legitimate class. Guidance blocks are authoring scaffolding deleted on fill, not artifact prose; and the gap is generic, since a target project mid-bootstrap can hold two unfilled skeletons whose guidance collides the same way.
  Date/Author: 2026-09-17, reviewer.

- Decision: (pre-execution review) M6's outward-reference failing case uses `tools/verify`, not `docs/PRINCIPLES.md`.
  Rationale: the rule as specified resolves a reference as `template/<reference>` first, and `template/docs/PRINCIPLES.md` exists — the originally prescribed violation was not a violation, and the milestone would have discovered that mid-demonstration. `tools/verify` resolves live and nowhere inside the payload, so it fails exactly one rule for exactly the stated reason.
  Date/Author: 2026-09-17, reviewer.

- Decision: (M1) `ARCHITECTURE.md` gains a fifth component entry, "Check layer", for `tools/`.
  Rationale: the Components section enumerates what lives where, and a directory of executables that decides this repository's invariants is not findable from an enumeration that stops at four. The plan's files-list already allots `ARCHITECTURE.md` "a component entry for `tools/`" without assigning it to a milestone; M1 is the milestone that creates the directory, so leaving the entry to a later one would publish a command in `AGENTS.md` whose owner the architecture does not name. The entry states the one boundary that matters — nothing under `tools/` is copied into `template/`, and a check states no invariant of its own — so a later reader cannot mistake a script for the specification.
  Date/Author: 2026-09-17, M1 session.

- Decision: (M1) a failing check prints a count line after its violation blocks, which M1's authoring-time transcript does not show.
  Rationale: the check protocol in Interfaces and Dependencies requires "one block per violation … then the count", and the expected transcript in Concrete Steps was written before any implementation existed. The contract governs; the transcript was an expectation. `plans-md-identity` therefore ends its failure output with `plans-md-identity: 1 violation in 1 pair compared.`, which also keeps failing output shaped like passing output — every line prefixed with the check name, so the aggregator's stream stays attributable.
  Date/Author: 2026-09-17, M1 session.

- Decision: (M1) `tools/verify` counts a check's exit 2 as a failure and names the missing script when a listed check is absent or non-executable.
  Rationale: the protocol says exit 2 is never a pass, and the recovery note in Idempotence and Recovery warns about the inverse state — a register row reading `enforced` while the aggregator does not run the check. A deleted or chmod-stripped script would otherwise make the aggregator print `2 of 2 checks passed` while running one, which is the failure mode the whole plan exists to remove.
  Date/Author: 2026-09-17, M1 session.

- Decision: (M2) `tools/checks/template-live-drift` prints one line per pair, and the one-line rule in the check protocol under Interfaces and Dependencies is amended to admit it.
  Rationale: the card's Acceptance and this milestone's acceptance clause 1 both require one line per pair naming the comparison applied, for the reason the card states — a silently skipped exclusion is how an exclusion list grows until the check covers nothing. The protocol's "exactly one line" was written for a check that examines a set and reports a total, and a specific card contract governs over a generic output shape. The cost is that a clean `./tools/verify` run prints 21 lines where it printed 3; the four cards still to be built all specify a single summary line, so the cost does not compound.
  Date/Author: 2026-09-17, M2 session.

- Decision: (M2) the exclusions are decided by path — `.gitkeep` by name, and any file under `template/docs/capabilities/` that is neither `index.md` nor a `*_FORMAT.md` — and an excluded file is out of every comparison, not only out of counterpart existence.
  Rationale: the card excludes card files from counterpart existence and says `index.md` is not excluded; `CARD_FORMAT.md` also has a live counterpart, so the discriminator is "neither the register nor a format document". Excluding a card from existence alone would leave `docs/capabilities/fast-verify.md` compared structurally against the generic card it instantiates, asserting a correspondence the card denies: instantiation is a rewrite, not a fill.
  Date/Author: 2026-09-17, M2 session.

- Decision: (M2) the byte-identical pair is a literal in the script, `BYTE_IDENTICAL="plans/PLANS.md"`.
  Rationale: which files are one file stored twice is a decision `ARCHITECTURE.md` makes, not a property of the tree, and there is nothing in the bytes to derive it from. A second zero-tolerance pair is a one-line edit beside a comment saying why the line exists.
  Date/Author: 2026-09-17, M2 session.

- Decision: (M2) prose repair extended to `docs/capabilities/fast-verify.md`, in the live half only.
  Rationale: its Remediation message quoted `plans-md-identity`, which this milestone deletes, and its passing-case sentence said the command prints one line per check, which the decision above makes false. Both sentences were written in M1 against this repository's own check set, so correcting them is not a generic improvement: `template/docs/capabilities/fast-verify.md` says "a one-line summary naming each check that ran" and is untouched, keeping this plan's payload edits to the two that M3 and M4 own.
  Date/Author: 2026-09-17, M2 session.

- Decision: (M2) the drift card's Enforcement point is rewritten to name `./tools/verify` and `tools/hooks/pre-commit` instead of continuous integration, and to record that its byte-identity comparison subsumed the check that was deleted.
  Rationale: the same reason as M1's enforcement-point decision — there is no continuous integration here to name — plus the old text described the pre-M1 state, "replaces the single `cmp` line that covers only the byte-identity case today", which this milestone makes false twice over.
  Date/Author: 2026-09-17, M2 session.

- Decision: (post-M2 review, routed to M7 rather than acted on) The two
  format documents are checked by subsequence where their class implies byte
  identity. `CARD_FORMAT.md` and `DECISION_FORMAT.md` are shipped-verbatim —
  a target project never edits a format it conforms to, and this repository's
  live copies are copies — yet the drift check compares each pair by heading
  subsequence, so prose drift between the copies of a format document is
  undetectable. Both pairs are byte-identical today, verified with `cmp`
  during review. The fix is one line per pair in `BYTE_IDENTICAL` plus the
  matching sentence in `docs/capabilities/template-live-drift.md`'s
  Invariant and in `ARCHITECTURE.md`'s correspondence invariant, but it
  changes the invariant of an `enforced` card, which requires its own
  failing-case demonstration — real work, not a review edit. M7 either does
  it under its close-out or records it as debt with the next free
  identifier.
  Date/Author: 2026-09-17, reviewer.

- Decision: (M3) references inside fenced and indented blocks are quoted material, not citations, and this becomes a fourth decidable legitimate class in both halves of the card.
  Rationale: a capability card's Remediation message section is required by `docs/capabilities/CARD_FORMAT.md` to hold the exact failure text, and `doc-integrity`'s failure text necessarily names a file that must not exist. Without the class, the live card reports two findings against itself the moment it lands (observed; see Surprises), and the alternative is two allowlist entries whose only content is that a card is a card — an allowlist that excuses the specification rather than a mention. The reason is universal: a target project instantiating this card gets the same remediation block, so under `ARCHITECTURE.md`'s rule that a generic improvement is incomplete until both halves carry it, `template/docs/capabilities/doc-integrity.md` gains the same class. That makes this plan's edits to the payload's `doc-integrity` card two clauses in one file rather than one, and the files list under `Interfaces and Dependencies` is amended to say so; the count of payload *files* this plan touches is unchanged at two.
  Date/Author: 2026-09-17, M3 session.

- Decision: (M3) the live card states its failing case with the offending path unbackticked in prose and backticked only inside the remediation block.
  Rationale: the quoted-material class makes the block safe, but prose is checked, so a card whose instruction reads "cite `docs/NOPE.md`" fails itself for a sentence rather than for a defect. Writing the path bare in the instruction and letting the indented block carry the exact shape keeps the card readable and keeps the check honest. This is a constraint on how any future card names a must-not-exist file, which is why it is logged rather than left in the file's style.
  Date/Author: 2026-09-17, M3 session.

- Decision: (M3) a missing allowlist file, and an allowlist entry with no reason, are both `cannot run` — exit 2 — rather than silently tolerated.
  Rationale: the allowlist format under `Interfaces and Dependencies` makes the one-line reason mandatory, and an unreasoned exception is exactly the state the card says an allowlist must never reach. Treating an absent file as an empty allowlist would instead convert three deliberate mentions into three violations and invite whoever hits them to re-add the entries without the reasons. Exit 2 names the file and the line, and `tools/verify` already counts 2 as a failure.
  Date/Author: 2026-09-17, M3 session.

- Decision: (M3) `docs/capabilities/fast-verify.md`'s remediation example is corrected from `1 of 2 checks failed` to `1 of 3`, in the live half only.
  Rationale: the same reason as M2's prose repair to that card — its example is a transcript of this repository's real aggregator, so a count that no longer matches the check list is a false statement in an `enforced` card's specification. `template/docs/capabilities/fast-verify.md` carries no count and is untouched. The count will need the same correction in M4, M5, M6 and M7; a session that adds a check and leaves the example stale has left the card describing a command that no longer exists.
  Date/Author: 2026-09-17, M3 session.

- Decision: (M3) `D3`'s Details paragraph is updated in place rather than left for M7 to delete.
  Rationale: it counted the instantiated payload cards — "one of the five" — and naming two of five is a one-line edit, while leaving it wrong for four more milestones makes the register and the debt row disagree about the same fact. `docs/DEBT.md`'s row for `D3` and its trigger cell are history and stay as written; M7 still deletes both.
  Date/Author: 2026-09-17, M3 session.

- Decision: (M4) the attribution class resolves a citation three ways — repository-relative, as a bare sibling filename, and, from inside `template/`, relative to `template/`.
  Rationale: a payload file citing `AGENTS.md` is citing `template/AGENTS.md`, because that is how every reference inside the payload resolves in the copy a target project receives; a checker that reads the citation from this repository's root sees no attribution and reports the pair (observed; see Surprises). Without the third form the same restatement is a finding in the payload and a legitimate class in the live half, which is an asymmetry that would have to be paid for with an allowlist entry excusing a correctly attributed sentence. The sibling form is there for the same reason one directory over: a file cites its neighbour by bare filename, and `docs/capabilities/index.md` is cited as `index.md` from inside that directory.
  Date/Author: 2026-09-17, M4 session.

- Decision: (M4) the `tools/` component entry in `ARCHITECTURE.md` is reworded so that an inventory stops phrasing itself like a card's Enforcement point.
  Rationale: the window `and the versioned hook tools hooks pre commit` was shared with all three cards that existed, and every card this plan still builds states its enforcement point the same way, so the alternative was an allowlist growing by one entry per card — the shape this plan already refused for the instantiated-card pairs. The fact is not duplicated: `ARCHITECTURE.md` owns what lives under `tools/` and a card owns where it is enforced. Only the phrasing collided, and the non-owning file for a card's enforcement point is the architecture.
  Date/Author: 2026-09-17, M4 session.

- Decision: (M4) the Per-stack hints of `docs/capabilities/doc-integrity.md` and `docs/capabilities/template-live-drift.md` now attribute the markdown-shell-git constraint to `ARCHITECTURE.md` instead of restating it.
  Rationale: both opened with "No toolchain applies: this repository is markdown operated on with shell and git", which is the cross-cutting invariant `ARCHITECTURE.md` owns, restated without naming its owner — precisely what `docs/PRINCIPLES.md` forbids and what the attribution class is for. Naming the owner in the same section is the repair the card's own remediation message offers, and it is cheaper than an allowlist entry per card. Both edits are live-half only: the payload's cards carry generic hints that never contained the sentence.
  Date/Author: 2026-09-17, M4 session.

- Decision: (M4) four pairs are carried as 20 per-window allowlist entries, and the two `docs/decisions/0016-skill-and-card-may-restate-one-fact.md` pairs are seeded with the live twin that record could not foresee.
  Rationale: the key format in `Interfaces and Dependencies` is one window per entry, and it stays that way — a pair-level key would silently excuse the next sentence someone copies between the same two files. Three pairs are the `0016` family: `skills/doc-garden/SKILL.md` against both copies of the `doc-integrity` card, and `skills/harness-init/SKILL.md` against `template/docs/capabilities/isolated-env.md`. The fourth is `ARCHITECTURE.md` against `docs/specs/bootstrap-flow.md`, which is the card's proper-noun-run class exactly: the six skill names in roster order, spelled out in one file to say where the procedures live and in the other to say what an outside reader receives.
  Date/Author: 2026-09-17, M4 session.

- Decision: (M4) `docs/MATURITY.md`'s Current rung section says in one sentence that `prose-duplication` is enforced and gates no rung.
  Rationale: that section counts the mechanical checks, so this milestone makes it stale either way, and a card that is `enforced` while appearing in no row of the Gating capabilities table reads as an omission unless the absence is stated. The card's reason is its own — a shared window locates a fact in two files without deciding which one owns it — so the sentence names the consequence rather than copying the reasoning.
  Date/Author: 2026-09-17, M4 session.

- Decision: (M5) `prose-duplication`'s attribution class gains a fourth citation form — a citation of either copy of a fact whose owner sits at the same relative path in both halves attributes the window against both copies — fixed in the check rather than allowlisted, with the live card's enumeration amended and the payload card untouched.
  Rationale: the pair the check reported was `docs/capabilities/evidence-check.md` against `template/AGENTS.md` while the card cites `AGENTS.md` correctly (observed; see Surprises). An allowlist entry there would have excused a correctly attributed restatement, which is the same reason M4 fixed the citing end of this class in the check. The payload's card states the class generically as "a backticked path to the file that owns the fact" and never enumerates the resolution forms, so the enumeration is the live specification of how a citation resolves here and the fix is live-half only — the generic sentence already covers an owner that a declared correspondence stores twice.
  Date/Author: 2026-09-17, M5 session.

- Decision: (M5) the timestamp remediation prints the shape `(YYYY-MM-DD HH:MMZ)` where the payload's card shows a concrete date.
  Rationale: the message asks the reader for the time they observed, and the first implementation handed them M1's real timestamp in the same line (observed; see Surprises) — a paste-ready date inside a request for an observation is the fabrication `AGENTS.md`'s working rule forbids and `evidence-check` explicitly cannot detect. The card carries the reason beside the message so a later reader does not "correct" it back to a date. This is a message-text choice in one instantiation, not a change to the invariant, so the payload card is untouched.
  Date/Author: 2026-09-17, M5 session.

- Decision: (M5) the check reports an empty-section failure with its own message rather than folding it into the missing-section message.
  Rationale: `template/docs/capabilities/evidence-check.md` states that an empty section is also a failure and publishes only two message shapes, one of which says so in its closing clause. A reader whose Decision Log heading is present and empty would be told the heading is missing, which is false at the moment it is read — the worst property a remediation line can have. Three shapes cost three lines in the card and remove the contradiction; the next action is identical in all three, which is what keeps it one invariant rather than two.
  Date/Author: 2026-09-17, M5 session.

- Decision: (M5) the check scopes the timestamp rule to the Progress section and reads headings only at column zero, with fenced and indented blocks skipped.
  Rationale: a plan quotes the convention's own skeleton — `plans/PLANS.md` shows `- [x] (2025-10-01 13:00Z) Example completed step.` inside an indented block — and quotes its own transcripts, so a checkbox or a heading inside quoted material is an example rather than a section or an entry. The same rule the sibling checks already apply to references and to windows applies here for the same reason, and it keeps the contract to the three things the card says are decidable without parsing the plan.
  Date/Author: 2026-09-17, M5 session.

- Decision: (M6) the skill layer's allowed targets are decided by resolving each reference inside `template/`, not by parsing or mirroring the enumeration in `ARCHITECTURE.md`; the enumeration is corrected to match and stays prose for a reader.
  Rationale: the card requires the edge set to be derived from file content rather than from a hand-maintained list, and this list had already drifted in three places before anyone built the check — which is exactly what a hand-maintained list does. Parsing the prose was the alternative and it is worse: the sentence names "the three `docs/` directories and their format documents", a phrase no parser decides, and a wording edit would silently change the rule. Resolving against the payload asks the payload what it ships, which is the question the rule is actually about, and it cannot go stale.
  Date/Author: 2026-09-17, M6 session.

- Decision: (M6) `ARCHITECTURE.md`'s plan-file paragraph now states that `plans/PLANS.md` is not a plan and names the rule's scope as `plans/active/` and `plans/completed/`.
  Rationale: 36 references from outside `plans/` name that path (observed; see Surprises), and the map's own allowed-target list already named it, so the rule as written contradicted the list it sits beside. A check has to resolve that contradiction one way or the other, and resolving it in the script would have put the scope of a rule somewhere other than the file that owns the rule.
  Date/Author: 2026-09-17, M6 session.

- Decision: (M6) a reference excludes any span containing a colon, and this check reads fenced and indented blocks that `doc-integrity` skips.
  Rationale: the two go together. Reading quoted material is what the rule needs — a dangling path inside a payload transcript still dangles for the reader of the copy, and the parent-directory allowance is what keeps an illustrative filename legitimate — but reading it also admits the allowlist key the payload's `doc-integrity` card quotes, which has a slash and no other excluded character (observed; see Surprises). A colon settles it without an allowlist entry: a file-and-line locator and a compound key are not paths, and nothing in either half writes a path with a colon in it. The consequence for a payload author is stated on the card: a foreign path that must appear goes in unbackticked, which is the rule M3 already logged for must-not-exist paths.
  Date/Author: 2026-09-17, M6 session.

- Decision: (M6) the payload rule carries a parent-directory allowance and the skill rule deliberately does not.
  Rationale: they answer different questions. A payload file may legitimately name a file the copy will not have, because a remediation transcript names the file its own failing case creates; the directory being shipped is what makes it an illustration rather than a dangling path. A procedure naming a file inside a shipped directory is the opposite case: the map's rule is precisely that a project's own card, spec, record or plan is not namable, and a parent-directory allowance there would excuse every one of them and leave the rule deciding nothing.
  Date/Author: 2026-09-17, M6 session.

- Decision: (M6) this check has no allowlist, and the mention scan skips a line already reported for its path reference.
  Rationale: nothing in the tree needs an exception — the first run was clean over 418 references — and the one case an allowlist would have covered, an illustrative foreign path, has a syntactic answer instead: write it unbackticked. An allowlist file created empty for symmetry with the sibling checks is a file whose first entry nobody has had to justify. The dedupe is the same instinct in the output: one word, one rule, one block, because the two scans see the same line from two directions (observed; see Surprises).
  Date/Author: 2026-09-17, M6 session.

## Outcomes & Retrospective

M1 through M6 are executed; M7 is not, so this section stays unwritten until then. What M1 fixes for that comparison: the budget published in `AGENTS.md` is 5 seconds against an observed clean run of 0.5 seconds, and the one defect found while building was in the check rather than in the tree — `cmp`'s missing line number on a prefix difference, recorded in Surprises. What M2 adds: the hand walk between the halves is gone from `docs/DEBT.md`, and building it found no defect in the tree at all — the authoring tally was confirmed rather than corrected, which is the first milestone here to report that. What M3 adds: the reference sweep that found a dangling path by hand at the close of v1 is now a command, the three references it reports are the three deliberate mentions authoring measured, and again no defect was found in the tree — the defect this milestone did find was in the card's own specification, which could not state its remediation message without a class the payload was missing. What M4 adds: the one-owner rule that a hand sweep enforced at the close of v1 is decided across both halves by a command, and this is the first milestone whose first run found defects in the tree rather than confirming a tally — three pairs closed by repairs in the non-owning file, one inventory sentence and two Per-stack hints paragraphs; one defect in the check itself; and four pairs carried as the judgement class the card declares. What M5 adds: the requirement that a plan carry its own record is no longer enforced only by the session that is running out of room to honour it, and the defect this milestone found was again in a check rather than in the tree — `prose-duplication` resolved a citation from the citing end but not to both copies of the cited owner, which the new card's own out-of-scope sentence exposed on the first run. What M6 adds: the layer map is no longer a set of rules a reader applies, and the defect it found was in the map rather than in the tree or in a check — the allowed-target list had drifted in three places and the plan-file rule contradicted its own list, both corrected in the file that owns them, with one clause of the map left undecidable and routed to `D10`. At M7 this section compares the result against the purpose above on four points — whether one command decides all six invariants, what each check does not decide, whether the time budget published in `AGENTS.md` held, and which of the defects found during building were pre-existing rather than introduced by this work.

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

`docs/capabilities/` holds the three cards this project wrote for itself — `template-live-drift`, `blueprint-eval`, `loop-runner` — plus the instantiated `fast-verify`, `doc-integrity`, `prose-duplication`, `evidence-check` and `boundary-lint`, `CARD_FORMAT.md`, and the register `index.md`. As of M6, six of the eight rows read `enforced` and `blueprint-eval` and `loop-runner` read `specced`; the register is where that is stated, and the sentence you are reading is orientation, not a second copy of it.

`template/docs/capabilities/` holds six generic cards the payload ships: `fast-verify`, `evidence-check`, `doc-integrity`, `boundary-lint`, `prose-duplication`, and `isolated-env`. Five of the six state invariants that hold in this repository too. `isolated-env` does not apply: there is no toolchain and no runtime here.

`AGENTS.md` publishes one command under Commands, `./tools/verify` with a 5-second budget, plus the one-time `git config core.hooksPath tools/hooks` install line. The two hand commands it published before M1 are now `tools/checks/scaffolding-markers` and, since M2, the byte-identity comparison inside `tools/checks/template-live-drift`. The marker check's pattern is written with the final letter of each marker word in brackets so that the script does not match itself; that trick, and its limit — it cannot tell a real unfilled slot from a marker quoted in prose — is `D2` in `docs/DEBT.md`.

`docs/DEBT.md` runs `D2` through `D10`. `D1` — the hand-checked correspondence between the two halves — was deleted in M2 when the check replaced it, and `D9` records the two questions that check cannot decide. `D3` is the uninstantiated generic cards; its Details paragraph has been kept current as each card landed and now reads all five instantiated, and M7 still deletes the row. `D10`, added in M6, carries the one layer-map clause no check can decide. `D6` (no brownfield ratchet for `boundary-lint`) and `D7` (the blueprint has never been run) are not this plan's work and stay.

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

The work. Normalize each artifact to its sequence of lowercase alphanumeric words after stripping fenced blocks, indented blocks, and HTML comment blocks, take every window of eight consecutive words, and report any window shared by two artifacts. HTML comments must be stripped alongside the other two quoted forms because the payload's skeletons each carry a verbatim guidance block by design — eight template files share the same marker-definition sentence, which is twenty-eight pair findings with no legitimate class if comments are windowed — and a guidance block is authoring scaffolding, not the artifact's prose; amend the quoted-material class in `template/docs/capabilities/prose-duplication.md` to say "indented blocks, fenced blocks, and HTML comment blocks" before instantiating it, so the live copy arrives correct. Eight is the floor the card sets and not a preference: shorter windows fire on ordinary English, longer ones miss a restated one-sentence rule. A single `awk` pass that maps each window to the files containing it decides every pair at once; the pairwise-intersection shape would be several hundred process invocations for the same answer.

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
2. Adding a backticked reference to `tools/verify` inside any file under `template/` and running `tools/verify` gives a nonzero exit naming the offending file, line, target, and the rule id violated — `tools/verify` resolves in this repository and nowhere inside the payload, which is exactly the outward reference the rule bans. Revert.
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

All commands run from the repository root; each script and each command below assumes it, and `cd "$(git rev-parse --show-toplevel)"` is how a script gets there rather than hard-coding a path. This section is updated as work proceeds. The M1 and M2 transcripts below are **observed**, copied from the sessions that executed them on 2026-09-17; the M3-through-M7 transcripts are **expected**, written at authoring time against an implementation that does not yet exist.

M1, in order:

    mkdir -p tools/checks tools/allow tools/hooks
    # author tools/checks/scaffolding-markers, tools/checks/plans-md-identity,
    # tools/verify, tools/hooks/pre-commit
    chmod +x tools/verify tools/checks/* tools/hooks/pre-commit
    time ./tools/verify

Observed output on a clean tree, `time ./tools/verify` reporting `real 0m0.500s`:

    plans-md-identity: ok — 1 pair compared byte for byte.
    scaffolding-markers: ok — 8 artifacts scanned, 0 markers found.
    fast-verify: 2 of 2 checks passed (0s).

The failing case, then the revert:

    printf 'drift\n' >> template/plans/PLANS.md
    ./tools/verify; echo "exit=$?"

Observed:

    plans-md-identity: template/plans/PLANS.md and plans/PLANS.md differ,
    first at line 175. These two files are byte-identical by construction.
    Copy the intended version over the other and re-run.
    plans-md-identity: 1 violation in 1 pair compared.
    scaffolding-markers: ok — 8 artifacts scanned, 0 markers found.
    fast-verify: 1 of 2 checks failed.
    Fix the violations reported above and re-run ./tools/verify.
    exit=1

    git checkout -- template/plans/PLANS.md
    ./tools/verify; echo "exit=$?"

restored the clean run and `exit=0`. The second check was demonstrated the same way — appending an HTML comment opening with the guidance marker word to `docs/DEBT.md` produced `scaffolding-markers: docs/DEBT.md:122:<!-- GUIDANCE: … -->`, the marker remediation line, and `scaffolding-markers: 1 marker found in 8 artifacts scanned.`; and moving `docs/specs/index.md` aside produced `scaffolding-markers: cannot run — docs/specs/index.md is in the checked set and does not exist.` with exit 2, which `tools/verify` counts as a failure. Both were restored and the clean run re-observed.

The hook, demonstrated rather than assumed:

    git config core.hooksPath tools/hooks
    printf 'drift\n' >> template/plans/PLANS.md
    git add template/plans/PLANS.md && git commit -m "should be refused"

Observed: the full `plans-md-identity` remediation text and `fast-verify: 1 of 2 checks failed.`, then

    pre-commit: commit refused — ./tools/verify reported the violations above.
    Fix them and commit again. Do not pass --no-verify: the bypass leaves the
    violation in history with nothing recording that a gate was skipped.

with `git commit` exiting 1 and `git log --oneline -1` still naming the previous commit. Restored with `git reset HEAD template/plans/PLANS.md && git checkout -- template/plans/PLANS.md`. The hook was then seen passing on this milestone's own two commits, which is the enforcement point firing in its own context rather than from a shell.

M2 through M6 each follow the same three steps: write `tools/checks/<card-name>`, add it to the check list in `tools/verify`, then run the card's passing case and every failing case it specifies, copying the real output into `Surprises & Discoveries`. The failing cases are quoted in each milestone's Acceptance above; run them exactly as written, and if a check does not fail on a violation, the check is wrong — do not adjust the card to match what was built.

M2's transcripts below are **observed**, copied from the session that executed it on 2026-09-17. The clean run of `./tools/checks/template-live-drift` prints one line per pair — three shapes of line, abbreviated here — and then the total:

    template-live-drift: AGENTS.md — subsequence, 3 headings.
    template-live-drift: docs/capabilities/boundary-lint.md — excluded
    (card file: the payload ships a starter register and a project keeps
    its own).
    template-live-drift: plans/active/.gitkeep — excluded (directory
    placeholder).
    template-live-drift: plans/PLANS.md — byte-identical.
    template-live-drift: ok — 19 files under template/: 1 byte-identical,
    10 by heading subsequence over 48 headings, 8 excluded.

The three failing cases, each run through `./tools/verify` and each followed by a restore and a re-observed clean run at exit 0. Heading, in which `grep -v '^## Register$'` rewrote `docs/capabilities/index.md`:

    template-live-drift: docs/capabilities/index.md is missing the heading
    `## Register`, which template/docs/capabilities/index.md declares. Add
    the heading to the live file, or remove it from the template if the
    section is genuinely gone — a generic change belongs in both halves.
    template-live-drift: 1 violation in 19 files under template/.
    fast-verify: 1 of 2 checks failed.
    exit=1

Byte identity, after `printf 'drift\n' >> template/plans/PLANS.md`:

    template-live-drift: template/plans/PLANS.md and plans/PLANS.md differ,
    first at line 175. These two files are byte-identical by construction.
    Copy the intended version over the other and re-run.

Counterpart existence, after `printf '# Example\n' > template/docs/EXAMPLE.md`, where the total line counts 20 files because the violating file is one of them:

    template-live-drift: template/docs/EXAMPLE.md has no live counterpart at
    docs/EXAMPLE.md. Either instantiate it for this project, or — if it is a
    card file or a directory placeholder — check it against the exclusions in
    docs/capabilities/template-live-drift.md.
    template-live-drift: 1 violation in 20 files under template/.

The enforcement point was confirmed in both directions. `git add template/docs/EXAMPLE.md && git commit -m "should be refused"` printed the counterpart-existence remediation text, then `pre-commit: commit refused — ./tools/verify reported the violations above.`, exited 1, and left `git log --oneline -1` naming the previous commit; and the hook was seen passing on this milestone's own two commits.

M3's transcripts below are **observed**, copied from the session that executed it on 2026-09-17. The passing case, run through the aggregator with the card and the check both in place:

    time ./tools/verify

    scaffolding-markers: ok — 8 artifacts scanned, 0 markers found.
    …the twenty template-live-drift lines…
    doc-integrity: ok — 274 of 277 references resolved in 38 artifacts,
    2 format documents skipped, 3 allowlist entries applied, 0 stale.
    fast-verify: 3 of 3 checks passed (0s).

with `real 0m0.158s` on the clean run before the prose repairs and `0m0.26s` for the pair of runs after them — an order of magnitude inside the 5-second budget `AGENTS.md` publishes, so the budget line needs no change in this milestone.

The failing case, added as a map line after `docs/PRINCIPLES.md` in `AGENTS.md`:

    - `docs/NOPE.md` — placeholder

    ./tools/verify; echo "exit=$?"

Observed, after the unchanged output of the two earlier checks:

    doc-integrity: AGENTS.md:17 references `docs/NOPE.md`, which does not
    exist. Create the file, correct the path, or — if the mention is
    deliberate — add `AGENTS.md:docs/NOPE.md` to
    tools/allow/doc-integrity.txt with a reason.
    doc-integrity: 1 violation in 276 references across 38 artifacts.
    fast-verify: 1 of 3 checks failed.
    Fix the violations reported above and re-run ./tools/verify.
    exit=1

The enforcement point was confirmed with the violation still in the tree. `git add AGENTS.md && git commit -m "should be refused"` printed that same remediation text, then

    pre-commit: commit refused — ./tools/verify reported the violations above.
    Fix them and commit again. Do not pass --no-verify: the bypass leaves the
    violation in history with nothing recording that a gate was skipped.

and `git log --oneline -1` still named the previous commit, `06b6326`. Restored with `git reset -q HEAD AGENTS.md && git checkout -- AGENTS.md`, after which the check printed its clean summary at exit 0 and `git status --porcelain` showed only this milestone's own files. The hook was then seen passing on this milestone's commits.

One note for a later session reading this transcript: the refusal was demonstrated through `git commit … | tail -20`, whose own exit status is the pipeline's last command, so the `echo "commit exit=$?"` beside it printed `0` and means nothing. What proves the refusal is `git log --oneline -1` naming the previous commit — not the echo.

M4's transcripts below are **observed**, copied from the session that executed it on 2026-09-17. The first run, with the card not yet written and `tools/allow/prose-duplication.txt` holding only its header comment, is the routing work of this milestone rather than a failing case:

    printf '# prose-duplication judgement-class exceptions.\n' > tools/allow/prose-duplication.txt
    time ./tools/checks/prose-duplication

    …eight violation blocks, abbreviated to their first clauses…
    prose-duplication: ARCHITECTURE.md and docs/capabilities/doc-integrity.md share the window
    "and the versioned hook tools hooks pre commit" (16 words shared in total). …
    prose-duplication: ARCHITECTURE.md and docs/specs/bootstrap-flow.md share the window
    "harness init plan author plan execute doc garden" (9 words shared in total). …
    prose-duplication: 8 duplicated pairs in 27769 windows across 41 artifacts.

    real 0m0.642s

Three of the eight were repaired in the non-owning file and one was a defect in the check, all four recorded in `Surprises & Discoveries` and the `Decision Log`; the remaining four were seeded into the allowlist as 20 window keys. The passing case after that, run through the aggregator with the card, the register row and the wiring all in place:

    time ./tools/verify

    scaffolding-markers: ok — 8 artifacts scanned, 0 markers found.
    …the twenty-one template-live-drift lines…
    doc-integrity: ok — 297 of 300 references resolved in 39 artifacts,
    2 format documents skipped, 3 allowlist entries applied, 0 stale.
    prose-duplication: ok — 42 artifacts compared, 29129 eight-word windows
    examined, 20 allowlist entries applied, 0 stale.
    fast-verify: 4 of 4 checks passed (0s).

with `real 0m0.397s`, an order of magnitude inside the 5-second budget `AGENTS.md` publishes, so the budget line needs no change in this milestone either.

The failing case, the sentence at `docs/PRINCIPLES.md` line 14 appended as a bullet under Working rules in `AGENTS.md` — a section that does not cite that file, which is what keeps the copy from being a legitimate attributed restatement:

    printf -- '- The rule: every fact lives in exactly one file, and every other file that\n  needs it links to that file instead of restating it.\n' >> AGENTS.md
    ./tools/verify; echo "exit=$?"

Observed, after the unchanged output of the three earlier checks:

    prose-duplication: AGENTS.md and docs/PRINCIPLES.md share the window "the
    rule every fact lives in exactly one" (24 words shared in total). One of
    the two owns this fact. Delete the copy, leave a reference to the owning
    file, name the owning file in the same section so the restatement reads as
    attributed, or — if the window is a proper-noun run, a path list or a
    section-name list both files must spell out — add
    "AGENTS.md:docs/PRINCIPLES.md:the rule every fact lives in exactly one" to
    tools/allow/prose-duplication.txt with a reason.
    prose-duplication: 1 duplicated pair in 29097 windows across 42 artifacts.
    fast-verify: 1 of 4 checks failed.
    Fix the violations reported above and re-run ./tools/verify.
    exit=1

The enforcement point was confirmed with the violation still in the tree. `git add AGENTS.md && git commit -m "should be refused"` printed that same remediation text, then the `pre-commit: commit refused` block, and `git log --oneline -1` still named the previous commit, `d3bca76`. Restored with `git reset -q HEAD AGENTS.md && git checkout -- AGENTS.md`, after which the check printed its clean summary at exit 0 and `git status --porcelain` printed nothing. The hook was then seen passing on this milestone's own commits.

M5's transcripts below are **observed**, copied from the session that executed it on 2026-09-17. The passing case, run through the aggregator with the card, the register row and the wiring all in place:

    time ./tools/verify

    scaffolding-markers: ok — 8 artifacts scanned, 0 markers found.
    …the twenty-one template-live-drift lines…
    doc-integrity: ok — 311 of 314 references resolved in 40 artifacts,
    2 format documents skipped, 3 allowlist entries applied, 0 stale.
    prose-duplication: ok — 43 artifacts compared, 30103 eight-word windows
    examined, 20 allowlist entries applied, 0 stale.
    evidence-check: plans/active/mechanical-gate-set.md — Progress, Surprises
    & Discoveries, Decision Log and Outcomes & Retrospective all present and
    non-empty; 4 of 7 Progress entries complete, every one timestamped.
    evidence-check: ok — 1 active plan checked, 4 sections required of each,
    7 Progress entries read.
    fast-verify: 5 of 5 checks passed (0s).

with `real 0m0.433s`, an order of magnitude inside the 5-second budget `AGENTS.md` publishes, so the budget line needs no change in this milestone either.

The first failing half, the missing section, produced by rewriting this plan through `grep -v '^## Decision Log$'`:

    ./tools/verify; echo "exit=$?"

Observed, after the unchanged output of the four earlier checks:

    evidence-check: plans/active/mechanical-gate-set.md is missing the
    required section `## Decision Log`.
      plans/PLANS.md requires Progress, Surprises & Discoveries, Decision Log
      and Outcomes & Retrospective in every plan. Add the heading and the
      entries the work produced; a heading with nothing under it is also a
      failure.
    evidence-check: 1 violation in 1 active plan, 7 Progress entries read.
    fast-verify: 1 of 5 checks failed.
    Fix the violations reported above and re-run ./tools/verify.
    exit=1

Restored from a copy taken before the edit, after which `./tools/verify` printed its clean summary at exit 0. The second failing half, the untimestamped entry, produced by ticking this plan's own M5 checkbox with `sed -i '' '23s/^- \[ \] M5/- [x] M5/'`:

    evidence-check: plans/active/mechanical-gate-set.md:23 — Progress entry
    "M5 — `evidence-check` instantiated, built and `enforced`; …" is marked
    complete but carries no timestamp.
      Completed entries record when the work was observed, as
        - [x] (YYYY-MM-DD HH:MMZ) M5 — `evidence-check` instantiated, built
        and `enforced`; …
      Use the time you observed, not an estimate.
    evidence-check: 1 violation in 1 active plan, 7 Progress entries read.
    fast-verify: 1 of 5 checks failed.
    exit=1

The enforcement point was confirmed with that violation still in the tree. `git add plans/active/mechanical-gate-set.md && git commit -m "should be refused"` printed the same remediation text, then the `pre-commit: commit refused` block, and `git log --oneline -1` still named the previous commit, `c7cad06`. Restored with `git reset -q HEAD plans/active/mechanical-gate-set.md && git checkout -- plans/active/mechanical-gate-set.md`, after which `./tools/verify` printed its clean summary at exit 0 and `git status --porcelain` showed only this milestone's own files. The hook was then seen passing on this milestone's own commit.

Two paths beyond the card's failing case were exercised and restored, because the card publishes a message for each. A throwaway second plan under `plans/active/` carrying one ticked entry without a stamp, an empty Surprises heading and no Outcomes heading printed all three message shapes and `evidence-check: 3 violations in 2 active plans, 8 Progress entries read.`; and moving this plan aside printed `evidence-check: ok — no active plans under plans/active/, 4 sections required of each.` at exit 0.

M6's transcripts below are **observed**, copied from the session that executed it on 2026-09-17. The rule-id block landed in `ARCHITECTURE.md` first, because the check refuses to run without it; the check was then written against it and run standalone before it was wired:

    ./tools/checks/boundary-lint

    boundary-lint: ok — 418 references examined in 58 files, 3 rules
    applied from ARCHITECTURE.md, no banned mention in 17 payload files
    or 6 procedures.

A check that passes on its first run has proved nothing, so each of the five violation shapes was produced with one appended line and then reverted. Abbreviated to the first clause of each block; every one carried its full remediation text and exited 1:

    printf 'A stray line naming `tools/verify`.\n' >> template/docs/DEBT.md
    boundary-lint: template/docs/DEBT.md:69 names `tools/verify`, which
    does not resolve inside the payload.

    printf 'This came from the blueprint.\n' >> template/GOALS.md
    boundary-lint: template/GOALS.md:109 mentions blueprint.

    printf 'See `docs/decisions/0001-greenfield-first.md`.\n' >> skills/retro/SKILL.md
    boundary-lint: skills/retro/SKILL.md:151 names
    `docs/decisions/0001-greenfield-first.md`, which the payload does not
    ship.

    printf 'Copy from `template/AGENTS.md`.\n' >> skills/plan-author/SKILL.md
    boundary-lint: skills/plan-author/SKILL.md:137 names
    `template/AGENTS.md`, which the payload does not ship.

    printf -- '- `plans/completed/v1-blueprint.md` — the v1 record\n' >> docs/DEBT.md
    boundary-lint: docs/DEBT.md:141 references
    `plans/completed/v1-blueprint.md`, a plan file that exists.

The three ways the map itself can fail the check, each followed by a restore and a re-observed clean run:

    git mv ARCHITECTURE.md ARCHITECTURE.md.bak
    boundary-lint: cannot run — the layer map ARCHITECTURE.md does not exist.

    grep -v '^    no-' ARCHITECTURE.md > … && cp … ARCHITECTURE.md
    boundary-lint: cannot run — ARCHITECTURE.md has no rule-id block under
    "## Layer map and dependency rules".

    sed -i '' 's|^    no-plan-file-dependency|    no-plan-dependency|' ARCHITECTURE.md
    boundary-lint: cannot run — ARCHITECTURE.md names rule
    `no-plan-dependency`, which tools/checks/boundary-lint does not implement.
    boundary-lint: cannot run — tools/checks/boundary-lint implements rule
    `no-plan-file-dependency`, which ARCHITECTURE.md does not name under
    "## Layer map and dependency rules".

Both drift directions are covered by that third mutation, which reports them at once: renaming an id leaves the new name unimplemented and the implemented name unnamed. A fourth mutation, splicing a second id onto the same line, printed the identical pair. All three failures exited 2, which `tools/verify` counts as a failure rather than a pass.

The passing case through the aggregator, with the card, the register row, the gating row and the wiring all in place:

    time ./tools/verify

    …the twenty-one template-live-drift lines…
    doc-integrity: ok — 340 of 343 references resolved in 41 artifacts,
    2 format documents skipped, 3 allowlist entries applied, 0 stale.
    prose-duplication: ok — 44 artifacts compared, 31657 eight-word windows
    examined, 20 allowlist entries applied, 0 stale.
    boundary-lint: ok — 440 references examined in 59 files, 3 rules applied
    from ARCHITECTURE.md, no banned mention in 17 payload files or 6
    procedures.
    fast-verify: 6 of 6 checks passed (1s).

with `real 0m0.505s`, an order of magnitude inside the 5-second budget `AGENTS.md` publishes, so the budget line needs no change in this milestone either.

The milestone's own failing case, the outward reference, run through the aggregator:

    printf -- '\nThe check this project runs is `tools/verify`.\n' >> template/docs/MATURITY.md
    ./tools/verify >/dev/null; echo "verify exit=$?"

Observed, after the unchanged output of the five earlier checks:

    boundary-lint: template/docs/MATURITY.md:159 names `tools/verify`, which
    does not resolve inside the payload.
      ARCHITECTURE.md no-outward-payload-reference: nothing under template/
      may name a path outside template/, because a target project receives
      that directory with no ancestor context and the reference dangles on
      arrival.
      Point it at an artifact the payload ships, or write it unbackticked if
      it is an illustration rather than a path. If the rule itself is wrong,
      change ARCHITECTURE.md in this same commit and say why in the plan's
      Decision Log.
    boundary-lint: 1 violation in 59 files, 441 references examined.
    fast-verify: 1 of 6 checks failed.
    verify exit=1

The enforcement point was confirmed with the violation still in the tree. `git add template/docs/MATURITY.md && git commit -m "should be refused"` exited 1, printed that same remediation text and then the `pre-commit: commit refused` block, and `git log --oneline -1` still named the previous commit, `602a813`. Restored with `git reset -q HEAD template/docs/MATURITY.md && git checkout -- template/docs/MATURITY.md`, after which `./tools/verify` exited 0 and `git status --porcelain` printed nothing. Acceptance clause 3 was then re-run through the aggregator: with the map renamed, `fast-verify: 5 of 6 checks failed.` at exit 1 — the other four checks name `ARCHITECTURE.md` in their own checked sets — and the rename was reversed with `git mv`, after which the clean run returned.

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

**Check protocol.** Exit 0 means the invariant holds, and the check's last line is `<check-name>: ok — <what was examined, with counts>`; a check whose card requires per-unit reporting prints one name-prefixed line per unit before it, which as of M5 is `template-live-drift` over the pairs under `template/` and `evidence-check` over the plans in flight. Exit 1 means a violation, and the check prints one block per violation built from its card's remediation text with the real offender substituted — file, line, target, rule — then the count; a check that reports per unit prints the unit's line only when that unit has no violation, so a unit is described once either way. Exit 2 means the check could not decide, and it prints `<check-name>: cannot run — <reason>` with the next action; `tools/verify` treats 2 as a failure and never as a pass. No check writes to the tree.

**Aggregator protocol.** `tools/verify` runs the checks in a fixed order written in the script — a literal list, never a glob, so that output order is stable — and streams each child's output unmodified, because a summary that swallows a remediation message fails `fast-verify`'s own acceptance. It ends with `fast-verify: <M> of <M> checks passed (<seconds>s).` and exit 0, or one line per failure plus `fast-verify: <N> of <M> checks failed.` and `Fix the violations reported above and re-run ./tools/verify.` and exit 1. The check list at the end of this plan's work is: `scaffolding-markers`, `template-live-drift`, `doc-integrity`, `prose-duplication`, `evidence-check`, `boundary-lint`.

**Allowlist format.** One entry per line: a key, two spaces, `#`, and a one-line reason. Blank lines and lines beginning with `#` are ignored. Keys are `<citing-path>:<reference>` for `doc-integrity` and `<fileA>:<fileB>:<window>` for `prose-duplication`, matching the remediation text each card publishes. Entries that match nothing are reported as a stale count in the passing summary line so they decay visibly; they do not fail the check, because the card states one invariant and this is not it.

**Time budget.** `AGENTS.md` publishes a budget in seconds beside the command, because an agent that cannot predict what a command costs stops running it. Set it in M1 to twice the observed clean run, rounded up to five seconds, and never above sixty. Each later milestone re-measures; if the number no longer holds, the budget line and the check set are corrected in that same commit.

**Identifiers already spent.** Debt rows `D1` through `D7` exist; this plan creates `D8` (M1), `D9` (M2), `D10` (M6) and deletes the `D1` and `D3` rows. Decision records run `0001` through `0016`; graduation in M7 starts at `0017`. Neither series reuses a number.

**Files this plan edits outside `tools/`.** `AGENTS.md` (Commands, and the hand-walk paragraph), `ARCHITECTURE.md` (a component entry for `tools/` — reworded in M4 so an inventory does not phrase itself like a card's enforcement point — the rule-id block, the allowed-target list, the instantiated-card clause, Known rough edges, and as M6 found, the plan-file rule's own scope, which contradicted the allowed-target list beside it), `GOALS.md` (Scope, one sentence in M7), `docs/DEBT.md` (`D2`'s location, `D8`/`D9`/`D10` added, `D1`/`D3` deleted, `D3`'s instantiation count kept current until it is), `docs/MATURITY.md` (Current rung, six gating rows, the closing paragraph, and one sentence in M4 stating that `prose-duplication` gates no rung), `docs/capabilities/index.md` (header prose and six rows), `docs/capabilities/template-live-drift.md` (what it does not decide, and as M4 found, its Per-stack hints attributing the markdown-shell-git constraint rather than restating it), `docs/capabilities/doc-integrity.md` (the same hints repair, on top of being one of the new card files), `docs/capabilities/prose-duplication.md` (one of the new card files, plus the fourth citation form M5 added to its attribution class), `docs/capabilities/fast-verify.md` (its remediation example's check count, re-measured in every milestone that adds a check), five new card files under `docs/capabilities/`, `template/docs/capabilities/doc-integrity.md` (the plan-file exclusion and, as M3 found, the quoted-material class) and `template/docs/capabilities/prose-duplication.md` (HTML comments join the quoted-material class) — the only two payload files this plan edits — `docs/specs/` (a new spec and its index row), and `docs/decisions/` (new records in M7).

**What this plan deliberately does not do.** It does not instantiate `isolated-env`, which would be a check that passes on everything in a repository with no toolchain. It does not build `blueprint-eval` or `loop-runner`, which need a live trial and an unattended loop that `docs/DEBT.md` `D5` and `D7` park. It does not claim L1 on the ladder: the promotion rule needs twenty consecutive green landed changes and a named human, and this plan produces neither. It does not add continuous integration, because there is no remote to add it to. It does not touch `D4` (skill auto-discovery) or `D6` (the brownfield ratchet for `boundary-lint`), both of which are payload questions that a real target project answers.

## Revision Notes

- 2026-09-17 (pre-execution review): amended M4's normalization to strip
  HTML comment blocks (with the template card gaining the same clause,
  making the payload edits two), replaced M6's failing-case reference with
  one that actually violates the rule, and corrected the files-list
  accordingly. Reason: both defects would have surfaced mid-milestone in an
  executing session — one as twenty-eight unexplained findings, one as a
  failing case that passes — which is exactly the class of hole authoring
  review exists to catch. Nothing else changed; milestone boundaries,
  acceptance counts, and contracts stand.

- 2026-09-17 (M1 execution): recorded M1 complete in `Progress`, replaced
  M1's expected transcripts in `Concrete Steps` with the output observed
  while running them, added three observations to `Surprises & Discoveries`
  and three decisions to the `Decision Log`. Reason: `plans/PLANS.md`
  requires the living sections to carry observed evidence rather than
  expectations, and two of the three observations change what a later
  milestone can assume — `cmp` alone cannot locate a prefix difference, and
  `tools/hooks/pre-commit` judges the working tree rather than the index.
  One authoring measurement in `Artifacts and Notes` is now stale by design:
  `core.hooksPath` is set in the clone this session ran in, which is the
  install line `AGENTS.md` publishes. Milestone boundaries, acceptance
  counts, and contracts are unchanged.

- 2026-09-17 (M2 execution): recorded M2 complete in `Progress`, added M2's
  observed transcripts to `Concrete Steps`, four observations to `Surprises &
  Discoveries`, and five decisions to the `Decision Log`; refreshed the three
  stale paragraphs under `Context and Orientation` → `What exists today`,
  which still described the pre-M1 command set and the now-deleted `D1`; and
  amended the check protocol under `Interfaces and Dependencies` to admit
  per-unit reporting. Reason: the card's Acceptance and M2's acceptance
  clause 1 both require one line per pair, which the protocol's "exactly one
  line" forbade — a conflict inside the plan that an executing session has to
  resolve in writing rather than silently. Milestone boundaries and
  acceptance counts are unchanged; the one contract that changed is named
  above with its rationale in the `Decision Log`.
- 2026-09-17 (post-M2 review): added one routed finding to the Decision
  Log — format-doc pairs are byte-copies by class but subsequence-checked
  by the drift card; M7 strengthens or debts it. Reason: an invariant
  weaker than its class's own rule is drift the check cannot see, and the
  decision to widen `BYTE_IDENTICAL` belongs to the artifact owners, not to
  a review.

- 2026-09-17 (M3 execution): recorded M3 complete in `Progress`, added M3's
  observed transcripts to `Concrete Steps`, four observations to `Surprises &
  Discoveries` and five decisions to the `Decision Log`; refreshed the two
  paragraphs under `Context and Orientation` → `What exists today` that
  counted instantiated cards and `D3`'s state; and amended the files list
  under `Interfaces and Dependencies` for two edits M3 made that the list did
  not allot — `docs/capabilities/fast-verify.md`'s stale check count and the
  second clause in the payload's `doc-integrity` card. Reason: the card could
  not state its own remediation message under the classes it declared, which
  is a hole in the specification rather than in the check, and a hole that
  every target project instantiating that card would hit. Milestone
  boundaries and acceptance counts are unchanged; the payload files this plan
  touches are still the same two.

- 2026-09-17 (M4 execution): recorded M4 complete in `Progress`, added M4's
  observed transcripts to `Concrete Steps`, five observations to `Surprises &
  Discoveries` and five decisions to the `Decision Log`; refreshed the two
  paragraphs under `Context and Orientation` → `What exists today` that
  counted enforced cards and `D3`'s state, and the `Outcomes &
  Retrospective` running summary; and amended the files list under
  `Interfaces and Dependencies` for three edits M4 made that the list did not
  allot — the rewording of the `tools/` component entry, the Per-stack hints
  repair in two live cards, and the sentence in `docs/MATURITY.md` saying
  this card gates no rung. Reason: the first run found eight pairs, and where
  a pair was a phrasing collision rather than a duplicated fact the repair
  belonged in the non-owning file, which pulled three files into this
  milestone that authoring had not foreseen. Milestone boundaries and
  acceptance counts are unchanged; the one contract that changed is the
  attribution class, which now resolves a payload citation the way the
  payload resolves it, with the rationale in the `Decision Log`. The payload
  files this plan touches are still the same two.

- 2026-09-17 (M5 execution): recorded M5 complete in `Progress`, added M5's
  observed transcripts to `Concrete Steps`, five observations to `Surprises &
  Discoveries` and four decisions to the `Decision Log`; refreshed the two
  paragraphs under `Context and Orientation` → `What exists today` that
  counted instantiated cards and `D3`'s state, and the `Outcomes &
  Retrospective` running summary; amended the check protocol under
  `Interfaces and Dependencies`, which named `template-live-drift` as the
  only per-unit reporter and now names two, and said nothing about what a
  per-unit check prints for a unit that has a violation; and amended the
  files list there for the second edit to `docs/capabilities/prose-duplication.md`
  and for the fifth gating row in `docs/MATURITY.md`. Reason: the new card's
  own out-of-scope sentence exposed a hole in the attribution class of the
  check M4 built — a citation resolved from the citing end but not to both
  copies of the cited owner — which is a defect in a sibling check rather
  than in this milestone's tree, fixed where M4 fixed the mirror of it.
  Milestone boundaries and acceptance counts are unchanged; the payload files
  this plan touches are still the same two, because the payload's card states
  the attribution class generically and never enumerates the forms.

- 2026-09-17 (M6 execution): recorded M6 complete in `Progress`, added M6's
  observed transcripts to `Concrete Steps`, seven observations to `Surprises &
  Discoveries` and five decisions to the `Decision Log`; refreshed the two
  paragraphs under `Context and Orientation` → `What exists today` that
  counted instantiated cards and named the debt range, and the `Outcomes &
  Retrospective` running summary; and amended the files list under
  `Interfaces and Dependencies` for the map edit this milestone made that the
  list did not allot — the plan-file rule's own scope. Reason: the layer map
  was wrong in two ways this plan had measured only one of. Its allowed-target
  list omitted the procedure directory as well as the two index files, and its
  plan-file rule contradicted its own list by forbidding what the list
  permits; a check binding to that map has to resolve both, and resolving
  either inside the script would have moved a rule out of the file that owns
  it. Milestone boundaries and acceptance counts are unchanged, no contract
  changed, and the payload files this plan touches are still the same two —
  the rule ids are this project's own layer map, and the payload states its
  map in a fill slot.
