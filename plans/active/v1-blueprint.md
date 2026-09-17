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
- [x] (2026-09-16) M3: Template payload — core artifacts. `template/AGENTS.md`,
  `template/GOALS.md`, `template/ARCHITECTURE.md`, `template/plans/PLANS.md`
  (byte-identical to live), `template/plans/{active,completed}/.gitkeep`.
  Forward references to `docs/` paths in the three skeletons are deliberate
  and resolve at M4; nothing in M3's own outputs is unresolved.
- [x] (2026-09-17 04:06Z) M4: Template payload — docs tree + capability
  spec-card format. `template/docs/{PRINCIPLES,MATURITY,DEBT}.md`,
  `template/docs/decisions/DECISION_FORMAT.md`,
  `template/docs/specs/index.md`,
  `template/docs/capabilities/{index.md,CARD_FORMAT.md}`. `MATURITY.md` is
  structure-plus-slots as specified; its L0–L3 text is M6's to write.
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
- Decision: Supersede the lean-PLANS.md backfill — adopt the OpenAI ExecPlan
  document verbatim as the base of `plans/PLANS.md`, with surgical edits
  only. Surgery performed: (1) heading de-branded ("Codex Execution Plans" →
  "Execution Plans") for harness portability; (2) the implementing
  instruction "do not prompt the user for 'next steps'; simply proceed to
  the next milestone" replaced with execute-one-milestone-then-stop,
  retaining resolve-ambiguities-autonomously and commit-frequently scoped
  within the milestone — an in-place edit so no contradictory instruction
  pair exists; (3) "Prototyping milestones" heading normalized from `#` to
  `##` (structural typo in source) and stray "—-" sequences normalized to
  "—"; (4) the skeleton's conditional "If PLANS.md is checked into the
  repo…" line replaced with a direct reference to `plans/PLANS.md`, which is
  always checked in here; (5) appended a marked House Rules section carrying
  the three local deltas — context hygiene (sizing, one-milestone-stop,
  split-on-overflow, plans-compose), evidence rules (observed-only,
  append-only corrections), lifecycle (active/completed, specs reflection,
  decision graduation) — with an explicit governing clause. Everything else,
  including the Formatting section (self-resolving for file-based plans) and
  the full skeleton, is verbatim.
  Rationale: Their wording is battle-tested prompt text; ours was untested
  synthesis. All of the earlier backfill's additions except the three House
  Rules deltas turned out to be subsumed by the adopted text (ambiguity
  resolution, prose-first, exact commands, Interfaces & Dependencies and
  Idempotence & Recovery via the skeleton, spike milestones). User call.
  2026-09-16.
- Decision: Two skeleton markers — `{{FILL: ...}}` for content slots, HTML
  comment blocks whose first word is `GUIDANCE` for authoring instructions,
  with guidance deleted once a file is filled.
  Rationale: M3 required a reader to tell placeholder from guidance at a
  glance, and both markers are mechanically greppable, so "is this template
  filled in?" becomes `grep -rn '{{FILL' <file>` plus `grep -rn GUIDANCE
  <file>` both empty rather than a judgement call. Guidance lives in HTML
  comments so a filled artifact renders clean without a deletion pass being
  required for correctness; guidance is nonetheless deleted on fill because
  authoring scaffolding in a shipped project file is per-session context tax.
  2026-09-16.
- Decision: Durable ownership boundaries live as real content in
  `AGENTS.md`'s Map (one line per artifact, naming what it owns); the
  per-file boundary/anti-pattern statements are authoring-time guidance.
  Rationale: An agent routing a new piece of knowledge needs one registry,
  not a scavenger hunt across eight file headers. `AGENTS.md` is already the
  entry point, so the map doubles as the ownership registry at zero extra
  context cost. The per-file guidance is for whoever fills that file and has
  no reader after that. 2026-09-16.
- Decision: `template/plans/PLANS.md` is a byte-identical copy of live
  `plans/PLANS.md`, not a symlink, include, or annotated variant.
  Rationale: The payload must survive a plain recursive copy into a target
  project on any filesystem, and skills may not depend on symlink support.
  Identity also makes this the one template file whose drift check is exact:
  `diff template/plans/PLANS.md plans/PLANS.md` must be empty, with no
  project-specific variance to reason about. 2026-09-16.
- Decision: (post-M3 review) Accept that `template/plans/PLANS.md` does not
  meet M3's acceptance clause "every template file states its ownership
  boundary and at least one anti-pattern" — the byte-identity decision
  correctly forbids annotating it, and identity is worth more than the
  header. M3 remains complete with this named carve-out. Process lesson for
  future sessions, and retro input for M10: when a decision narrows a
  milestone's written acceptance, say so against the acceptance explicitly
  in the same session — an unnamed deviation makes completion claims
  unauditable. Also from review: use date+time Progress timestamps per the
  skeleton (`2026-09-16 14:00Z`), not bare dates — rates of progress are
  unmeasurable otherwise. 2026-09-16/reviewer.
- Decision: Format-defining files (`docs/capabilities/CARD_FORMAT.md`,
  `docs/decisions/DECISION_FORMAT.md`) ship as plain prose with no
  `{{FILL}}` slots and no GUIDANCE comment blocks; their ownership
  boundary and anti-pattern are real content under a "What this directory
  owns" heading.
  Rationale: First drafts used the skeleton convention, which made both
  files permanently fail the fill check — `grep -c '{{FILL'` was 1 and
  `grep -c GUIDANCE` was 5 on files that are never filled in. The options
  were an exception list or files that need no exception. An exception list
  has to be carried by the M6 `template-live-drift` card, the M8 doc-garden
  procedure, and the M9 `harness-init` fill step, so it would be three
  copies of the same carve-out; prose costs nothing and keeps the fill
  check total. These files also differ in kind from the skeletons: a target
  project keeps them verbatim forever, so their guidance has a permanent
  reader and must not be deleted on fill. 2026-09-17.
- Decision: Capability card status lives only in
  `docs/capabilities/index.md`; card files carry no status field, and the
  failing-case evidence that promotes a card to `built` is recorded in the
  plan that built it, not in the card.
  Rationale: Status is the one fact about a card that changes, and a fact
  with two homes goes stale in one of them — the index is what
  `MATURITY.md` gates read, so the index wins. Keeping evidence in plans
  preserves the card as a specification: a card that accumulates
  observation transcripts stops being a one-page contract an in-project
  agent can implement cold. 2026-09-17.
- Decision: `docs/decisions/` gets no index file; records are discovered by
  their `NNNN-short-slug.md` filenames.
  Rationale: `capabilities/index.md` and `specs/index.md` exist because
  they own something the files cannot — mutable status, and behavior
  routing. A decision record is immutable once written and its filename is
  its summary, so an index would carry zero facts and one drift surface.
  2026-09-17.
- Decision: `PRINCIPLES.md`'s three example principles live inside its
  GUIDANCE comment block rather than in the body as marked examples.
  Rationale: M4 asked for examples "marked as examples"; the marker
  convention already has a mechanism for "not project content", and using
  it means the examples disappear in the same deletion pass as the rest of
  the guidance. Examples in the body would need a separate removal step
  that the fill check cannot see, which is exactly how a template's sample
  content ends up shipped as a project's real rules. 2026-09-17.
- Decision: `DEBT.md` is a register table plus an optional per-item
  Details section, and closing an item deletes its row rather than marking
  it done.
  Rationale: A table alone cannot satisfy GOALS.md's "enough context to
  pick up cold", and prose alone gives no at-a-glance inventory; the split
  lets cheap items stay one row. Deletion on close keeps the file's reading
  cost proportional to live debt — a register of resolved entries taxes
  every future reader identically to real debt, and version control already
  holds the history. 2026-09-17.
- Decision: (post-M4 review) This plan was authored under the pre-adoption
  lean convention; audited against the adopted `plans/PLANS.md` and found
  conformant — all four mandatory living sections present, Purpose first,
  Milestones distinct from Progress, revision notes maintained, contracts
  in Interfaces & Dependencies. The skeleton-only sections it lacks (Plan
  of Work, Concrete Steps, Artifacts and Notes) are covered by the
  Milestones narrative and evidence-in-Surprises, as the 2026-09-16
  adoption revision note already records; empty ceremonial sections will
  not be added. Rule going forward: a change to `plans/PLANS.md` triggers a
  conformance audit of every plan in `plans/active/`, recorded in each
  plan's Decision Log; doc-garden owns this check (M8 scope amended).
  Rationale: both executed milestones already ran under the adopted text —
  the exposure was authoring-time only — but the audit currently happened
  because a human worried, and a rule that fires on worry is not a rule.
  2026-09-17/reviewer.

## Surprises & Discoveries

- Observation: `plans/PLANS.md` needed no generalization for the template.
  M3 specified the skeleton as the live file "with blueprint-specific
  references removed"; there were none. The adopted ExecPlan text speaks
  only of "this repository", `plans/active/`, `plans/completed/`,
  `docs/specs/`, and `docs/decisions/` — all of which are template-provided
  paths, so every reference resolves inside any instantiation.
  Evidence: `grep -cniE "blueprint|harness|skills/|template/|GOALS\.md|
  ARCHITECTURE\.md" template/plans/PLANS.md` → `0`; `cmp plans/PLANS.md
  template/plans/PLANS.md` → silent (bytes equal).
- Observation: Guidance comments dominate the skeletons by line count, but
  the artifact a target project keeps is small, so the ~100-line `AGENTS.md`
  cap is not endangered by verbose authoring guidance.
  Evidence: `sed '/^<!--$/,/^-->$/d' <file> | grep -c .` → AGENTS.md 34,
  GOALS.md 23, ARCHITECTURE.md 22 non-blank lines; raw files are 96, 108,
  and 104 lines.
- Observation: The skeleton marker convention does not fit every template
  file. Files that a target project keeps verbatim (the two format-defining
  files) are never "filled", so authoring markers in them turn the fill
  check into a permanent false positive.
  Evidence: first drafts measured `fill:1 guidance:5` for both
  `CARD_FORMAT.md` and `DECISION_FORMAT.md` via `grep -c '{{FILL'` /
  `grep -c GUIDANCE`; after the rewrite both read `fill:0 guidance:0`,
  while the five skeleton files read `fill:2–14 guidance:6–10`. The
  template now has two file classes, recorded as a contract in Interfaces &
  Dependencies.
- Observation: M4 closed the forward references M3 left open — every map
  entry in `template/AGENTS.md` now resolves inside the payload.
  Evidence: extracting the map's backticked paths and testing each with
  `[ -e ]` from `template/` printed `OK` for all ten captured entries
  (`GOALS.md`, `ARCHITECTURE.md`, `docs/PRINCIPLES.md`,
  `docs/MATURITY.md`, `docs/DEBT.md`, `docs/decisions/`, `docs/specs/`,
  `docs/capabilities/`, `plans/PLANS.md`, `plans/active/`); the regex
  missed `plans/completed/` because it shares a line with `plans/active/`,
  and that directory exists with its `.gitkeep`.
- Observation: The docs skeletons carry no reference to this repository, so
  no generalization pass was needed for them either.
  Evidence: `grep -rnoiE "harness-blueprint|blueprint|skills/|template/|
  v1-blueprint" template/docs` → no matches.
- Observation: The payload survives a plain recursive copy into an empty
  directory with the two file classes exactly as contracted — eight
  skeletons carrying markers, three files carrying none.
  Evidence: `T=$(mktemp -d) && cp -R template/. "$T/"` then per-file
  `grep -c '{{FILL'` / `grep -c GUIDANCE` in `$T` →  `AGENTS.md 8/9`,
  `ARCHITECTURE.md 14/10`, `GOALS.md 13/11`, `docs/DEBT.md 5/8`,
  `docs/MATURITY.md 14/10`, `docs/PRINCIPLES.md 7/7`,
  `docs/capabilities/index.md 3/6`, `docs/specs/index.md 2/6`, and `0/0`
  for `docs/capabilities/CARD_FORMAT.md`,
  `docs/decisions/DECISION_FORMAT.md`, and `plans/PLANS.md`; thirteen files
  copied including both `plans/{active,completed}/.gitkeep`, and
  `cmp "$T/plans/PLANS.md" plans/PLANS.md` was silent.

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
  the owning `docs/capabilities/index.md` and nowhere else — card files
  carry no status field. A card may not reach `built` without a
  demonstrated failing case showing its remediation message; that evidence
  is recorded in the plan that built it, not in the card (M4).
- **Card format**: defined by `template/docs/capabilities/CARD_FORMAT.md`
  (M4) with sections Invariant, Enforcement point, Acceptance, Remediation
  message, Per-stack hints. All M6 cards conform to it.
- **Template ↔ live correspondence**: every file under `template/` has a
  same-relative-path live counterpart at repo root; structure identical,
  only project-specific content varies.
- **Claude shim**: root `CLAUDE.md` containing exactly `@AGENTS.md` (M9).
- **Skeleton markers**: `{{FILL: ...}}` marks a content slot; every HTML
  comment block in a template file begins with the word `GUIDANCE` and is
  authoring instruction to be deleted on fill. A template file is fully
  filled when `grep -rn '{{FILL' <file>` and `grep -rn GUIDANCE <file>` are
  both empty. M4's docs skeletons and M9's `harness-init` procedure depend
  on this convention (M3).
- **PLANS.md identity**: `template/plans/PLANS.md` and live `plans/PLANS.md`
  are byte-identical; `diff` between them must be empty. This is the
  zero-tolerance case of template ↔ live correspondence and the simplest
  check the M6 `template-live-drift` card must cover (M3).
- **Two template file classes** (M4): *skeletons* carry `{{FILL}}` slots and
  GUIDANCE blocks and are filled on instantiation (`AGENTS.md`, `GOALS.md`,
  `ARCHITECTURE.md`, `docs/PRINCIPLES.md`, `docs/MATURITY.md`,
  `docs/DEBT.md`, `docs/specs/index.md`, `docs/capabilities/index.md`);
  *shipped-verbatim* files carry neither marker and are kept as-is by a
  target project (`docs/capabilities/CARD_FORMAT.md`,
  `docs/decisions/DECISION_FORMAT.md`, `plans/PLANS.md`). The fill check in
  the Skeleton markers contract therefore needs no exception list: a
  shipped-verbatim file reads zero for both greps from the start. Anything
  M9's `harness-init` or M8's doc-garden says about filling applies to the
  skeleton class only.
- **Card file layout** (M4): one card per file at
  `docs/capabilities/<card-name>.md`, lowercase-hyphenated, one invariant
  each, with a matching row in `docs/capabilities/index.md` whose columns
  are `Card | Status | Enforced at`.
- **Decision record format** (M4): `docs/decisions/NNNN-short-slug.md`,
  numbers never reused, exactly four sections — Decision, Rationale, Date,
  Status — with Status one of `accepted`, `superseded by NNNN`, `reverted`.
  Records are append-only: superseding edits only the old record's Status.
  No index file exists in that directory. M5's decision graduation and M8's
  retro routing depend on this.
- **Specs reflection rule** (M4): stated in `docs/specs/index.md`. A plan
  does not move to `plans/completed/` until whatever a caller can now
  observe is written into a spec file there and listed in that file's
  index; a plan with a purely internal outcome reflects nothing and says so
  in its own Outcomes section. Spec files are present-tense and overwritten
  freely.
- **MATURITY.md section shape** (M4, filled at M6): headings Current rung;
  Rungs with `### L0`–`### L3`; Promotion rule; Demotion rule; Gating
  capabilities as a table with columns `Rung | Gating card | What it must
  subsume`. M6 replaces the slots and keeps the headings, in both the
  template and live copies.
- **DEBT.md shape** (M4): a Register table with columns
  `ID | Item | Where | Why deferred | Trigger to pay it down`, IDs of the
  form `D<n>` never reused, plus an optional per-item Details section for
  anything not pick-up-able from its row. Closing an item deletes its row
  and section. M10's deferred-work entries use this shape.

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
lacking specs reflection, active plans nonconformant with the current
`plans/PLANS.md` (mandatory sections, evidence discipline, revision notes),
DEBT.md staleness, broken cross-links, and (in this repo) template↔live
structural divergence; open smallest-possible fixes.
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

## Revision Notes

- 2026-09-16: Added the Interfaces & Dependencies section and a Decision Log
  entry after review flagged the lean convention's gaps versus the OpenAI
  ExecPlan document. Reason: cross-milestone contracts must live in the plan
  for fresh-context restarts.
- 2026-09-16: `plans/PLANS.md` replaced — OpenAI ExecPlan text adopted
  verbatim as base with surgical edits plus appended House Rules; see the
  superseding Decision Log entry for the full surgery list. Reason: adopt
  battle-tested wording while preserving this repository's fresh-context
  loop, evidence, and lifecycle rules. This plan already conforms: all
  mandatory living sections exist, and its Milestones narrative covers the
  skeleton's Plan of Work / Concrete Steps roles.
- 2026-09-16: M3 executed. Added two Interfaces & Dependencies contracts
  (skeleton marker convention, template ↔ live PLANS.md byte identity),
  three Decision Log entries, and two Surprises observations. Reason: the
  marker convention and the PLANS.md identity rule are relied on by M4, M6,
  and M9, so they must be readable from the plan alone by a fresh-context
  session. No milestone scope changed.
- 2026-09-17: M4 executed. Added six Interfaces & Dependencies contracts
  (two template file classes, card file layout, decision record format,
  specs reflection rule, MATURITY.md section shape, DEBT.md shape),
  amended the card-status contract to say status has exactly one home and
  evidence lives in plans, added five Decision Log entries and three
  Surprises observations. Reason: M5 fills the live docs tree from these
  skeletons, M6 writes cards and MATURITY.md content against these shapes,
  and M8/M9 procedures branch on the skeleton-versus-shipped-verbatim
  distinction — none of which is recoverable from the template files alone
  by a fresh-context session. No milestone scope changed. M4's written
  acceptance is met without carve-out: `CARD_FORMAT.md` states the
  failing-case and remediation bar for `built`, `specs/index.md` carries
  the reflection rule under its own heading, and all seven docs files name
  their owner boundary.
- 2026-09-17: Post-M4 review — audited this plan against the adopted
  `plans/PLANS.md` (it was authored under the earlier lean convention),
  found it conformant, and added the convention-change audit rule with
  doc-garden as its owner; M8's doc-garden scope amended accordingly.
  Reason: convention changes with plans in flight must trigger a mechanical
  audit, not depend on someone noticing.
