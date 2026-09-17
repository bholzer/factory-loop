# Build the v1 harness blueprint

This ExecPlan is a living document maintained per `plans/PLANS.md`. The
sections Progress, Decision Log, Surprises & Discoveries, and Outcomes &
Retrospective must be kept current as work proceeds.

## Purpose

After this plan completes, an agent in any of three harnesses (Claude Code,
Codex, omp) can be pointed at a new project, invoke `harness-init`, and get a
right-sized working harness: knowledge artifacts, a plan convention with
evidence discipline, capability spec cards for its mechanical enforcers, six
portable skills, and day-one entropy management. To see it working: copy
`template/` into an empty directory, follow any skill's procedure by hand, and
observe that every referenced artifact exists and every rule is executable
from repo content alone.

## Progress

- [x] (2026-09-16) Seed contract: `GOALS.md`, `AGENTS.md` stub, `plans/PLANS.md`.
- [x] (2026-09-16) This plan authored.
- [ ] M3: Template payload — core artifacts.
- [ ] M4: Template payload — docs tree + capability spec-card format.
- [ ] M5: Instantiate live repo from template.
- [ ] M6: Capability spec cards + MATURITY.md.
- [ ] M7: Skills — plan-author, plan-execute.
- [ ] M8: Skills — doc-garden, retro, capability-build.
- [ ] M9: Skill — harness-init + portability glue.
- [ ] M10: Paper verification, first retro, close plan.

## Decision Log

Decisions settled during the design conversation that produced this plan.
Recorded here so the reasoning exists in-repo; durable ones graduate to
`docs/decisions/` at M5.

- Decision: Thin-first; maturity ladder documented, not built.
  Rationale: Thin handles harness/stack agnosticism; autonomy is earned as
  mechanical enforcement replaces procedural gating. 2026-09-16.
- Decision: Meta-repo layout — `template/` payload, root = live instantiation.
  Rationale: Root-level template collides with the repo's own live
  AGENTS.md/GOALS.md/plans. Meta-repo makes dogfooding the sync test; this
  repo is client #1 of its own bootstrap flow. 2026-09-16.
- Decision: Include `GOALS.md`; fold `PROMPTS.md` into skills.
  Rationale: Non-goals, success conditions, and a grading anchor for
  retro/doc-garden have no other home. Reusable workflow entry points are
  what skills are. 2026-09-16.
- Decision: Include `docs/specs/` as an empty slot with a reflection rule.
  Rationale: Completed plans are change archives, not living behavior docs;
  without a specs home, behavior truth rots inside `plans/completed/`.
  2026-09-16.
- Decision: Evidence lives inside plans; no standalone build log in v1.
  Rationale: Fewer files, one source of truth per unit of work. A separate
  log is a maturity-step promotion. 2026-09-16.
- Decision: Single ExecPlan-style plan convention; reject the five-file
  GOALS/PROMPTS/build/context/log split.
  Rationale: The split's ownership hygiene is kept as rules inside PLANS.md;
  at thin scale separate files are duplication surface. 2026-09-16.
- Decision: Six-skill roster — harness-init, plan-author, plan-execute,
  capability-build, doc-garden, retro.
  Rationale: One skill per flywheel edge (bootstrap, plan, execute, enforce,
  GC, meta-learn), no overlap. plan-author/plan-execute split because
  planning and implementation are separately authorized activities and
  execution discipline is where drift is most expensive. 2026-09-16.
- Decision: capability-build is a standalone skill, not part of harness-init.
  Rationale: init runs once; capability building recurs forever — it is the
  specced→built→enforced edge that powers the maturity ladder. 2026-09-16.
- Decision: Mechanical enforcers are specced, never implemented, by the
  blueprint. Spec cards carry invariant, enforcement point, observable
  acceptance, mandatory remediation-message requirement, per-stack hints.
  Rationale: Mechanisms differ per stack; the blueprint guides an in-project
  agent on HOW to build them. 2026-09-16.
- Decision: Context hygiene rules — milestone sized to one fresh-context
  session, one milestone per invocation then stop, split-on-overflow, plans
  compose don't nest.
  Rationale: The plan file is durable context; the window is a disposable
  cache. Loop machinery reduces to stop rules + restart contract; no
  orchestration code. 2026-09-16.
- Decision: `loop-runner` (unattended outer loop) specced and parked at L2.
  Rationale: v1 is the watching phase — human-invoked iteration is where
  failure domains get caught. 2026-09-16.
- Decision: Portability via lowest common denominator — `AGENTS.md` read
  natively by Codex/omp, `CLAUDE.md` shim containing `@AGENTS.md` for Claude
  Code; skills as `skills/<name>/SKILL.md` with plain name/description
  frontmatter; skill bodies use only files, shell, git.
  Rationale: User switches harnesses; markdown + git is the whole
  portability story. 2026-09-16.
- Decision: Verification is paper-only for v1; live trial specced as the
  `blueprint-eval` capability card.
  Rationale: User call; the want is recorded as a card so it is repo-legible.
  2026-09-16.
- Decision: Greenfield-first; no brownfield machinery, but the bootstrap flow
  (orient → map → propose → fill) must work unchanged on a non-empty repo.
  Rationale: Brownfield just means ARCHITECTURE.md and DEBT.md start full.
  The hard part (boundary-lint retrofit) is deferred and tracked. 2026-09-16.
- Decision: Backfill `plans/PLANS.md` against the OpenAI ExecPlan doc after
  user review flagged its leanness. Adopted: decisions-made-in-plan,
  prose-first, exact-commands-with-expected-output, Interfaces & Dependencies
  and Idempotence & Recovery as required sections, spike milestones.
  Deliberately rejected: proceed-through-milestones-without-stopping
  (contradicts one-milestone-stop), chat-envelope fencing rules (our plans
  are files), repetition-as-emphasis (a fat convention is a per-session
  context tax).
  Rationale: Their doc optimizes single long-context runs; ours optimizes
  fresh-context loops. Gaps real, bulk not. 2026-09-16.

## Surprises & Discoveries

- None yet.

## Outcomes & Retrospective

- To be written at completion.

## Context & Orientation

This repository is currently four files: `GOALS.md` (project boundaries — read
it first), `AGENTS.md` (entry-point map), `plans/PLANS.md` (the plan
convention this document conforms to), and this plan. Nothing else exists yet.

Terms used below:

- **Payload / template**: the directory `template/` whose contents get copied
  into a target project and filled in. Skeleton files there contain authoring
  guidance and placeholders, not final prose.
- **Live instantiation**: this repository's own root artifacts, produced by
  filling in the template skeletons for *this* project. Divergence between
  template and live files is a bug in one of them.
- **Capability spec card**: a one-page markdown spec for a mechanical
  enforcer (e.g. a doc-link linter). Cards are stack-agnostic; an in-project
  agent implements the mechanism in whatever stack it finds. Card statuses:
  `specced` → `built` → `enforced` (wired into CI/hooks).
- **Skill**: a directory `skills/<name>/` containing `SKILL.md` with
  `name`/`description` YAML frontmatter — the format Claude Code, Codex, and
  omp all discover. Bodies are procedures over files, shell, and git only.
- **Maturity ladder**: L0 bootstrap (human gates everything) → L1
  self-verifying → L2 agent-reviewed → L3 bounded autonomous classes.
  Promotion rule: a human gate retires only when a mechanical check subsumes
  it and has been green for N cycles.

## Interfaces & Dependencies

Contracts that milestones establish and later fresh-context sessions rely on:

- **Skill layout**: `skills/<name>/SKILL.md`, YAML frontmatter with exactly
  `name` (lowercase-hyphenated, matching the directory) and `description`.
  Bodies reference `plans/PLANS.md` and other repo files; procedures use
  only files, shell, and git.
- **Capability card statuses**: `specced` → `built` → `enforced`, recorded in
  the owning `docs/capabilities/index.md`. A card may not reach `built`
  without a demonstrated failing case showing its remediation message.
- **Card format**: defined by `template/docs/capabilities/CARD_FORMAT.md`
  (M4) with sections Invariant, Enforcement point, Acceptance, Remediation
  message, Per-stack hints. All M6 cards conform to it.
- **Template ↔ live correspondence**: every file under `template/` has a
  same-relative-path live counterpart at repo root; structure identical,
  only project-specific content varies.
- **Claude shim**: root `CLAUDE.md` containing exactly `@AGENTS.md` (M9).

## Milestones

### M3: Template payload — core artifacts

Create `template/` with skeleton `AGENTS.md`, `GOALS.md`, `ARCHITECTURE.md`,
`plans/PLANS.md`, and empty `plans/active/`, `plans/completed/`. The skeleton
`PLANS.md` is the live `plans/PLANS.md` generalized (blueprint-specific
references removed). Skeletons carry brief authoring guidance as markdown
comments or clearly marked placeholder sections: what the file owns, what it
must not absorb, size limits (AGENTS.md ≤ ~100 lines).

Acceptance: every template file states its ownership boundary and at least
one anti-pattern it exists to prevent; skeleton `PLANS.md` contains no
reference to this blueprint repo; a reader can tell placeholder from
guidance at a glance.

### M4: Template payload — docs tree + spec-card format

Create `template/docs/`: `PRINCIPLES.md` (golden-principles skeleton with
2–3 example principles marked as examples), `MATURITY.md` (structure only —
content lands M6), `DEBT.md` (tracker table skeleton), `decisions/` (ADR
micro-format: Decision/Rationale/Date/Status), `specs/index.md` (with the
reflection rule: completed plans reflect behavioral outcomes here),
`capabilities/index.md` (status table: card / status / enforcement point),
and `capabilities/CARD_FORMAT.md` — the spec-card template with sections:
Invariant, Enforcement point, Acceptance (observable, including a
demonstrated failing case), Remediation message (mandatory), Per-stack hints
(optional).

Acceptance: `CARD_FORMAT.md` requires a failing-case demo and remediation
text for any card to reach `built`; `specs/index.md` states the reflection
rule; every docs file names its owner boundary.

### M5: Instantiate live repo from template

Fill in this repo's own `docs/` tree and `ARCHITECTURE.md` from the template
skeletons, and reconcile the seeded `AGENTS.md`/`GOALS.md`/`plans/PLANS.md`
against their template counterparts — differences are template bugs or seed
bugs; fix whichever is wrong and log which. Graduate durable entries from
this plan's Decision Log into `docs/decisions/`. Update `AGENTS.md`'s map to
reflect what now exists.

Acceptance: every template skeleton has a live counterpart; a diff walk
between template and live files shows only project-specific content varying,
structure identical; `docs/decisions/` contains the graduated decisions;
this milestone's reconciliation findings are in Surprises & Discoveries.

### M6: Capability spec cards + MATURITY.md

Write the five generic cards into `template/docs/capabilities/`:
`doc-integrity` (cross-links resolve, required sections present, freshness),
`boundary-lint` (no dependency edge violates the declared layer map),
`fast-verify` (one cheap always-runnable verification command),
`isolated-env` (fresh isolated environment in one command, conflict-free
concurrent instances), `evidence-check` (active plans have current living
sections; no planned-command-as-evidence). Write the blueprint's own cards
into live `docs/capabilities/`: `blueprint-eval` (live trial: bootstrap a toy
repo in ≥2 harnesses, drive one feature through the loop), `loop-runner`
(unattended outer loop), `template-live-drift` (template ↔ live structural
divergence detection). All cards status `specced`. Then fill `MATURITY.md`
(template + live): L0–L3 definitions, the promotion rule, and which cards
gate which rung.

Acceptance: all eight cards conform to `CARD_FORMAT.md` including remediation
text and observable acceptance; both `capabilities/index.md` files list their
cards with status `specced`; `MATURITY.md` references only cards that exist.

### M7: Skills — plan-author, plan-execute

Create `skills/plan-author/SKILL.md` and `skills/plan-execute/SKILL.md`.
plan-author: create/revise ExecPlans conforming to `plans/PLANS.md`; refuse
milestones whose acceptance can't be stated as a few observable checks;
never begins implementation. plan-execute: fresh-context assumption; read
AGENTS.md, the named plan, and files the plan names; execute exactly one
milestone; update living sections with observed evidence only; split-in-place
on overflow; hard stop. Both defer to `plans/PLANS.md` as the contract rather
than duplicating it.

Acceptance: frontmatter is plain name/description; bodies contain no
harness-specific tool names; each skill states its stop condition and what it
must never do; no rule text duplicated from PLANS.md (references instead).

### M8: Skills — doc-garden, retro, capability-build

Create three skills. doc-garden: scan for docs↔code drift, completed plans
lacking specs reflection, DEBT.md staleness, broken cross-links, and (in this
repo) template↔live structural divergence; open smallest-possible fixes.
retro: reconstruct intended vs. actual from plan evidence; route each
material lesson to exactly one owner — doc, principle, capability card,
skill, or no-change — with an explicit materiality test. capability-build:
take one card from `docs/capabilities/`, inspect the project's stack, build
the enforcer with remediation messages, demonstrate a violating change
failing with remediation text visible, flip index status.

Acceptance: same portability checks as M7; capability-build's procedure
requires the failing-case demo before status may change; retro's owner list
includes "no change" and forbids multi-owner routing. Split this milestone
into two sessions if it won't fit one.

### M9: harness-init + portability glue

Create `skills/harness-init/SKILL.md`: orient in the target repo (works on
empty or non-empty), interview for goals/scope/non-goals, right-size (small
project ⇒ fewer artifacts — state the rules), copy and fill `template/`,
install the other five skills, write the `CLAUDE.md` shim containing
`@AGENTS.md`, and finish by reporting what was created and what was
deliberately omitted. Add this repo's own `CLAUDE.md` shim. Verify all six
skills' frontmatter/layout against Claude Code, Codex, and omp discovery
conventions; record the check results in this plan.

Acceptance: harness-init's procedure references only files that exist in the
final template; right-sizing rules are explicit (what gets omitted and when);
`CLAUDE.md` shim present at repo root; portability check results recorded as
observed evidence.

### M10: Close the loop

Paper verification: walk the full bootstrap flow against an imagined
greenfield target, checking each skill's procedure against the artifacts it
touches; log gaps found and fix small ones in place. Run doc-garden's
procedure manually over this repo. Write the first retro entry, routing
lessons from building the blueprint to owners. Record deferred work in
`docs/DEBT.md` (loop-runner build, brownfield boundary-lint retrofit, live
blueprint-eval trial). Reflect outcomes into `docs/specs/`, complete
Outcomes & Retrospective, move this plan to `plans/completed/`.

Acceptance: paper-walk findings logged with fixes noted; DEBT.md contains
the deferred items with enough context to pick each up cold; this plan lives
in `plans/completed/` with all living sections final.

## Validation & Acceptance

Overall: from a checkout of this repo, a reader following `AGENTS.md` can
reach every artifact; `template/` copied into an empty directory yields a
coherent skeleton where every cross-reference resolves; each skill is
executable by hand by a novice using only repo content; all eight capability
cards are implementable without asking questions this repo could have
answered. Formal live-trial validation is out of scope for v1 and specced as
`blueprint-eval`.

## Idempotence and Recovery

All milestones are additive file creation or in-place markdown edits;
re-running a milestone overwrites its own outputs and nothing else. If a
session dies mid-milestone, Progress shows the split state; restart from the
plan alone per `plans/PLANS.md`.
