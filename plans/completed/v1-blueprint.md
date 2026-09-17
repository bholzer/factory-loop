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
- [x] (2026-09-17 04:32Z) M5: Live repo instantiated from the template.
  Written as live content: `ARCHITECTURE.md`, `docs/PRINCIPLES.md`,
  `docs/DEBT.md`, `docs/specs/index.md`, `docs/capabilities/index.md`.
  Copied verbatim: `docs/capabilities/CARD_FORMAT.md`,
  `docs/decisions/DECISION_FORMAT.md`. Reconciled: `AGENTS.md` (map,
  new Commands section, working rules) plus created `plans/completed/` and
  `skills/`, each holding a `.gitkeep`. Graduated thirteen decisions to
  `docs/decisions/0001`–`0013`. Carve-out named against this milestone's
  acceptance: live `docs/MATURITY.md` is the unfilled skeleton copy,
  because M6 owns its content in both copies — the live fill check
  therefore reports 24 marker hits in that one file and none anywhere else.
- [x] (2026-09-17 04:52Z) M6: Capability spec cards + MATURITY.md. Five
  generic cards in `template/docs/capabilities/` — `fast-verify`,
  `evidence-check`, `doc-integrity`, `boundary-lint`, `isolated-env`; three
  of this project's own in live `docs/capabilities/` —
  `template-live-drift`, `blueprint-eval`, `loop-runner`. Both registers
  list their cards at `specced`; `MATURITY.md` filled in both copies;
  `ARCHITECTURE.md`, `docs/DEBT.md` (`D1` retargeted, `D3` added) and
  `AGENTS.md` reconciled; decisions `0014`–`0015` graduated. Acceptance met
  without carve-out.
- [x] (2026-09-17 05:06Z) M7: Skills — plan-author, plan-execute.
  `skills/plan-author/SKILL.md` and `skills/plan-execute/SKILL.md` written,
  `skills/.gitkeep` deleted, and the two live artifacts that claimed the
  skill layer was empty reconciled — `ARCHITECTURE.md`'s Skill layer entry
  and a new `skills/` line in `AGENTS.md`'s map. Acceptance met without
  carve-out.
- [x] (2026-09-17 05:22Z) M8: Skills — doc-garden, retro, capability-build.
  `skills/doc-garden/SKILL.md`, `skills/retro/SKILL.md` and
  `skills/capability-build/SKILL.md` written, and `ARCHITECTURE.md`'s Skill
  layer entry — the one live claim that four of the six procedures did not
  exist — reconciled to name five. The milestone was executed in one
  session; the authored option to split it into two was not needed.
  Acceptance met with one named substitution against its own text: the
  template ↔ live sweep the milestone assigns doc-garden "(in this repo)"
  is stated as a sweep of whatever structural correspondence
  `ARCHITECTURE.md` declares, because the skill layer's reference rule
  forbids a skill body from naming `template/`.
- [x] (2026-09-17 05:38Z) M9: Skill — harness-init + portability glue.
  `skills/harness-init/SKILL.md` written with an explicit right-sizing
  section, root `CLAUDE.md` added holding exactly `@AGENTS.md`,
  `ARCHITECTURE.md` reconciled (six skills; how the bootstrap procedure
  resolves the two things it may not name), `AGENTS.md` given the shim's map
  line, and the portability check set run over all six skills. Acceptance met
  without carve-out. The milestone's own verification step produced one
  finding beyond its acceptance — the installed procedures are outside every
  harness's auto-discovery root — registered as `D4` and covered by the
  skill's step 9 rather than left in the plan.
- [x] (2026-09-17 06:24Z) M10: Paper verification, first retro, close plan. The
  bootstrap flow was walked against a real greenfield copy rather than an
  imagined one (`cp -R template/. $T/` plus the six procedures), which found
  one overclaim in `skills/harness-init/SKILL.md` and fixed it in place.
  `doc-garden`'s eight sweeps were run over this repository: one dangling
  reference repaired in decision record `0015`, eight duplicated-prose pairs
  removed, `D1`'s trigger sharpened, `D3` recorded as overdue with evidence,
  and the remaining findings dispositioned. Deferred work added as `D5`–`D7`.
  The retro routed four material lessons — `docs/PRINCIPLES.md` gained the
  attribution clause, `docs/decisions/DECISION_FORMAT.md` (both copies) gained
  the reference-repair clause, the payload gained a sixth card
  `prose-duplication`, and `0016` graduated — with two no-changes recorded
  against the question they failed. `docs/specs/bootstrap-flow.md` written and
  indexed. Acceptance met with one named deviation: the sweep's fixes landed
  as one commit rather than one commit per finding, recorded below.

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
- Decision: M5 graduates thirteen decisions rather than waiting for plan
  completion, using the test "does this constrain how future work is done,
  and does its rationale have no other owning file". Graduated:
  `0001` meta-repo layout, `0002` PLANS.md adoption, `0003`
  one-milestone-per-session, `0004` two template file classes, `0005`
  PLANS.md byte identity, `0006` enforcers specced not implemented, `0007`
  card status single home, `0008` portability lowest common denominator,
  `0009` six-skill roster, `0010` autonomy by subsumption, `0011` no
  decisions index, `0012` evidence lives in plans, `0013` convention change
  triggers a plan audit. Deliberately not graduated: the scope choices whose
  fact `GOALS.md` already owns with its reason inline (greenfield-first,
  no `PROMPTS.md`, paper-only v1 verification, `loop-runner` parked at L2),
  and the authoring-time craft decisions whose only reader was the session
  that made them (PRINCIPLES examples living inside a guidance block,
  DEBT.md's table-plus-details shape, the post-M3 acceptance carve-out, the
  post-M4 conformance audit finding).
  Rationale: `plans/PLANS.md` graduates decisions at completion, but M5 is
  written to graduate them now, and the reason the milestone is right is
  that M6 through M10 are the first consumers — a card written at M6 needs
  `0007`'s status rule, and a skill written at M7 needs `0003` and `0008`.
  Waiting would mean those sessions reading a plan's log for rules that
  bind work beyond the plan. The stated test is what keeps graduation from
  becoming a wholesale copy: of the 26 entries this log held before M5, 13
  graduated and 13 stay. 2026-09-17.
- Decision: Decision records cite no plan path — origin is recorded in the
  Date field as "v1 blueprint design conversation", not as
  `plans/active/v1-blueprint.md`.
  Rationale: this milestone wrote the layer rule that artifacts never
  reference a specific plan file, and the reason applies with force here:
  every such path breaks when the plan moves to `plans/completed/`, which
  is guaranteed to happen and would silently invalidate thirteen records at
  once. Attribution without a path costs a reader one search and cannot
  rot. 2026-09-17.
- Decision: Live `docs/capabilities/index.md` and `docs/specs/index.md` are
  filled now with empty-register prose, while live `docs/MATURITY.md` ships
  as an unfilled skeleton copy until M6.
  Rationale: the split follows what can be said truthfully today. Both
  index files' content is a statement about what they contain, and "nothing
  yet, here is what a row will look like" is that statement — writing it
  removes them from the fill check permanently. `MATURITY.md`'s slots are
  rung definitions and a promotion rule, which cannot be honestly
  summarized as "empty"; an invented placeholder would be worse than a
  marker, because a marker is visibly unfinished and prose is not.
  2026-09-17.
- Decision: `ARCHITECTURE.md` declares the skill layer as a component and
  states its dependency rule before any skill file exists, marking the
  directory as empty in the entry itself.
  Rationale: the rule's whole job is to constrain the first skill written,
  so writing it at M7 would be writing it after its only chance to prevent
  the mistake. Declaring an empty component is honest as long as the
  emptiness is stated, which it is; the alternative — a rough-edge entry
  plus a debt row — would carry plan state inside an artifact that is not
  allowed to reference plans. 2026-09-17.
- Decision: The live `AGENTS.md` divergences from the template are seed
  bugs and were fixed in the live file; the template was left untouched.
  Rationale: the seed predates the template by two milestones, and every
  difference was a template section the seed simply lacked (Commands), a
  rule it lacked (the map-not-encyclopedia cap), or a conditional made
  obsolete by this milestone ("once `docs/decisions/` exists"). Nothing in
  the template was wrong, so nothing there changed — the first
  correspondence reconciliation resolved entirely in one direction.
  2026-09-17.
- Decision: (post-M5 review) `ARCHITECTURE.md`'s correspondence invariant
  corrected from "identical headings in identical order" to the subsequence
  form. The strict phrasing was false on the tree as committed — live files
  add headings where filled content is itself headed, and fill-slot
  headings differ by construction — and was contradicted by `D1` and this
  plan's own structural-correspondence contract, both of which the same
  session wrote. An invariant stated stronger than reality teaches readers
  to discount invariants. 2026-09-17/reviewer.
- Decision: Capability cards are project content, not skeletons: the payload
  ships a starter register a project prunes by deletion, so card files under
  `template/docs/capabilities/` are excluded from counterpart-existence
  correspondence, as are `.gitkeep` placeholders.
  `docs/capabilities/index.md` is not excluded. Graduated as `0014`.
  Rationale: the alternative that keeps correspondence total is
  instantiating all five shipped cards live, and `isolated-env` in a
  repository with no toolchain would be a check that passes on everything —
  a manufactured register row to satisfy a structural rule. The `.gitkeep`
  exclusion came from the walk finding `plans/active/.gitkeep` unmatched,
  which is placeholder semantics rather than drift. 2026-09-17.
- Decision: `MATURITY.md` ships with rung definitions, promotion rule, and
  demotion rule as final content; the only fill slot left in the template
  copy is Current rung, and the Gating capabilities table ships filled with
  the five shipped cards. Graduated as `0015`.
  Rationale: the plan said "fill `MATURITY.md` (template + live)", which is
  ambiguous between filling both with the same generic ladder and inventing
  a project position in the payload. The ladder is the blueprint's opinion
  and an opinion left as a slot is not shipped; the current rung is the only
  part that is genuinely per-project. One slot remains so the file stays in
  the skeleton class and the mechanical fill check still covers it.
  2026-09-17.
- Decision: Each card states one decidable invariant, which narrowed two of
  the five generic cards against this plan's own milestone text.
  `doc-integrity` is referential integrity only — the milestone's
  parenthetical also named "required sections present" and "freshness";
  sections moved to `evidence-check`, freshness is named in the card as not
  decidable from text and left to review. `evidence-check` checks section
  presence and timestamps on completed entries; the convention's
  "never report a planned command as passing evidence" is named in the card
  as a judgement no checker can make and stays a working rule in
  `AGENTS.md`. Deliberate non-resolving references are handled by an
  allowlist of exact `path:reference` pairs rather than a heuristic.
  Rationale: `CARD_FORMAT.md` forbids two invariants per card because such a
  card cannot report one actionable failure, and it warns specifically
  against checks built from a vague wish. An honest card that says what it
  does not cover routes the leftover to a named owner; a card that promises
  freshness detection gets built as something that greps timestamps and is
  then trusted. The allowlist choice follows the same logic: a checker that
  reports legitimate references is switched off, and then nothing is
  checked. Not graduated — each narrowing is stated in the card that owns
  it. 2026-09-17.
- Decision: This project's live register holds only its own three cards; the
  four generic invariants that do apply here stay prose enforced by reading,
  tracked as `docs/DEBT.md` `D3`, and the live L1 gate is
  `template-live-drift` alone. The two own cards whose milestone
  descriptions were capabilities rather than invariants were restated as
  decidable properties: `loop-runner` as a commit-shape stop rule (one
  Progress entry advanced per iteration, halt on unobserved acceptance),
  `blueprint-eval` as sufficiency of a plain copy to carry one feature
  through the loop in every supported harness.
  Rationale: instantiating four more cards here would have doubled the
  milestone while nothing is enforced in either half, so the gap is recorded
  where gaps are picked up cold rather than half-closed. The restatements
  were forced by `CARD_FORMAT.md`: "an unattended outer loop" and "a live
  trial" are wishes, and a card whose Invariant section is a wish cannot
  have an acceptance that gates promotion. 2026-09-17.
- Decision: A skill body carries only what a procedure needs — when to
  invoke, what to read, the steps, an explicit refusal list, and a stop
  condition — and non-duplication against `plans/PLANS.md` is checked
  mechanically by word-shingle overlap rather than by reading.
  Rationale: M7's acceptance forbids duplicating rule text, and "did I
  restate a rule?" is exactly the judgement a tired author gets wrong. The
  check found the failure it was built for on the first run: the refusal
  bullet against nested plans had reproduced the convention's own sentence
  verbatim across ten overlapping six-word windows, and the rewrite states
  the refusal while pointing at the convention for the alternative. The
  refusal and stop sections are the part a skill genuinely owns — the
  convention says what a plan must be, and the skill says what the agent
  running the procedure must not do. Not graduated: the practice is a
  verification technique with no reader beyond the sessions writing skills,
  and M8 is the next one. 2026-09-17.
- Decision: Skills name payload-provided artifacts and directories, never a
  project-specific file inside them; the layer rule in `ARCHITECTURE.md` was
  sharpened to say so.
  Rationale: the rule as written allowed `docs/**`, which reads as
  permission to cite a decision record — and `plan-author`'s revision
  section wanted to cite the record that owns the convention-change audit.
  That citation would resolve here and dangle in every target project, which
  is the same defect the payload's outward-reference ban exists to prevent,
  one layer over. The sharpening belongs in `ARCHITECTURE.md` rather than in
  a decision record because the file already owns the skill layer's
  dependency rule and a second home for it would be the drift this project
  keeps writing rules against. 2026-09-17.
- Decision: `plan-author` describes how a convention-change audit is
  recorded and explicitly disclaims owning its trigger.
  Rationale: the trigger is already owned — `doc-garden` performs the check —
  so restating it in a skill would put one rule in two files, while omitting
  the procedure entirely would leave the auditing session inventing where to
  write the outcome. Splitting on trigger-versus-procedure keeps both facts
  single-owner, and the disclaimer is what stops a future reader from
  treating the silence as an oversight and "fixing" it. 2026-09-17.
- Decision: `skills/.gitkeep` was deleted in the same commit that added the
  first skill.
  Rationale: a placeholder's only job is to keep an empty directory in git,
  and this repository already wrote that rule down twice — as the
  `.gitkeep` exclusion in the drift card and as the principle that content
  with no reader left is removed rather than annotated. 2026-09-17.
- Decision: `doc-garden` states its structural-correspondence sweep as
  "whatever `ARCHITECTURE.md` declares", not as a template ↔ live check.
  Rationale: the milestone text assigns the skill a template ↔ live sweep
  "(in this repo)", and a skill body may not name `template/` — the same
  rule M7 sharpened after a decision-record citation nearly shipped
  dangling. The alternatives were a harness-blueprint-only paragraph, which
  breaks the rule outright, and dropping the sweep, which would leave the
  one invariant this project most needs swept unowned. Pointing at the
  declaring artifact keeps the skill portable and makes the sweep stronger
  than the milestone asked: any correspondence a project declares gets
  walked, in this project's case the counterpart, subsequence, and byte
  identity rules. It also puts the comparison method where it already lives,
  since `ARCHITECTURE.md` states the subsequence form that a naive diff gets
  wrong. 2026-09-17.
- Decision: The convention-change audit is split the way M7 predicted:
  `doc-garden` owns the trigger and the audit's scope, and points at
  `skills/plan-author/SKILL.md` for where inside a plan the outcome is
  recorded.
  Rationale: `plan-author` already disclaims the trigger and describes the
  recording, so the two skills now form a closed pair with neither fact
  duplicated. Leaving the pointer out was the alternative, and it would have
  left an auditing session knowing it must record an outcome with no
  statement of where — which is how the outcome ends up in a commit message.
  2026-09-17.
- Decision: `retro`'s materiality test is three questions — will it recur,
  does it cost less than it saves, can it be stated as something checkable
  or applicable — all three required, with the failed question recorded on
  every no-change.
  Rationale: the milestone requires an explicit materiality test and a
  "no change" owner, and an unrecorded no-change is indistinguishable from
  an oversight, so the next retro re-examines the same difference cold. The
  third question is the one that does the work: it is what rejects "be more
  careful", which is the lesson a retro produces by default and the one no
  reader can act on. 2026-09-17.
- Decision: `capability-build`'s procedure is a numbered ten-step sequence
  where the M7 and M8 sibling skills are narrative, and repairing an
  unbuildable card is explicitly outside it.
  Rationale: the steps are a proof obligation rather than advice — a failing
  case observed before the check was wired, or a status flipped before the
  failing case, proves nothing — and order is exactly what prose loses. The
  card-repair exclusion follows the card scope rule: a card whose invariant
  is a wish has no acceptance that can gate promotion, so the pass stops and
  reports rather than quietly narrowing the invariant to whatever it managed
  to build. 2026-09-17.
- Decision: (post-M8 review) `doc-garden`'s scaffolding sweep no longer
  attributes the marker definition to `AGENTS.md` — post-fill, no AGENTS.md
  defines the markers anywhere, since the defining guidance blocks are
  deleted on fill. Reworded to note markers are self-describing where they
  survive. A portable skill's claims must be true in a bootstrapped
  project, not only in this repository; the portability check set missed
  this because it checks paths and vocabulary, not attributions.
  2026-09-17/reviewer.
- Decision: (post-M8 review, routed to M10 retro rather than acted on) The
  skill reference-scope rule structurally forces fact duplication between
  portable skills and project cards: `doc-garden`'s reference sweep carries
  the three legitimate-miss classes that `doc-integrity` also specifies,
  because a skill body may not cite a prunable card file. The duplication
  is currently verbatim-free (shingle-clean) but conceptual, and one copy
  will drift. M10's retro should weigh owners: the classes could live in
  `CARD_FORMAT.md`-adjacent shipped-verbatim documentation both may cite,
  or the sweep could defer to "whatever the register's built cards decide"
  the way the correspondence sweep defers to `ARCHITECTURE.md`.
  2026-09-17/reviewer.
- Decision: M9 settles the payload-naming conflict by parameterization, not
  by exemption: `harness-init` resolves the payload directory through the
  `AGENTS.md` map of the checkout it was invoked from, and resolves the
  native-entry-point filename through the environment it is running in.
  `ARCHITECTURE.md` now states this as a consequence of the existing rule
  rather than as a carve-out from it.
  Rationale: the two shapes M8 recorded were an `ARCHITECTURE.md` exemption
  and a phrasing that never writes the path. Exemption loses on two counts.
  It is a carve-out the portability check set would have to carry as an
  allowlist entry — the same cost the M4 format-file decision refused when
  it chose prose over an exception list — and it would ship a literal path
  that resolves in exactly one checkout, which is the defect the rule
  exists to prevent, not an exception to it. Parameterization also reuses
  the M8 shape for project-specific facts inside portable skills: name the
  artifact that declares the fact, not the fact. Not graduated to
  `docs/decisions/`: the rule and its one-sentence reason now live in
  `ARCHITECTURE.md`, which owns dependency rules, so a record would be a
  second owner. Next free record number stays `0016`. 2026-09-17.
- Decision: The harness entry-point shim stays out of the payload;
  `harness-init` writes it, and this repository's own `CLAUDE.md` is a
  live-only file with no template counterpart.
  Rationale: which filename a harness reads natively, and whether one is
  needed at all, is a property of the environment running the session, not
  of the project being bootstrapped — a payload copy would install one
  harness's entry point into every project unconditionally, including
  projects whose agent reads `AGENTS.md` directly. Correspondence runs
  template → live only, so a live-only root file is not drift. The cost is
  that the payload cannot be verified to contain a working shim; that is
  acceptable because the shim's content is one reference line and step 9 of
  the procedure states it. 2026-09-17.
- Decision: The auto-discovery finding is registered as `D4` and covered by
  a step in `harness-init`, rather than being fixed by moving the canonical
  skill location or superseding
  `docs/decisions/0008-portability-lowest-common-denominator.md`.
  Rationale: what was verified is that the file shape is the intersection of
  the three harnesses' conventions; what was falsified is only that the
  *location* is. Reading a procedure by its mapped path works in every
  harness today, so nothing is broken — only automatic surfacing is absent.
  Moving the location or adding per-harness configuration are both changes
  to what the payload installs, and choosing between them from documentation
  alone is the kind of guess the paper-only verification scope was accepted
  to defer; `blueprint-eval` is where it gets answered. `0008` is not
  superseded because its decision is about file shape and content, which
  held. 2026-09-17.
- Decision: The paper walk was run as a real copy rather than against an
  imagined target. The milestone says "an imagined greenfield target"; the
  session instead created the target — `cp -R template/. $T/` plus the six
  procedure directories — and resolved every backticked path inside it.
  Rationale: an imagined walk can only check the claims the walker thinks to
  check, and the expensive failures are the ones nobody imagines. The copy
  costs one command and turns "every reference resolves" from a judgement
  into an observation, which is the same reason this project writes acceptance
  as commands. Not graduated: the technique belongs to the card that specifies
  the payload trial, and `docs/capabilities/blueprint-eval.md` already states
  a stronger version of it. 2026-09-17.
- Decision: The duplication sweep's fixes landed as one commit rather than one
  commit per finding, against `skills/doc-garden/SKILL.md`'s own disposition
  rule.
  Rationale: eight of the twelve findings were one defect class — a pointer
  restated as prose — and six of the eight fixes were in artifacts the same
  milestone was already editing for other reasons, so per-finding commits
  would have produced a chain of commits none of which could be reverted
  independently anyway. Stated as a deviation rather than quietly done: the
  rule is right for a gardening pass that runs on its own, and this pass ran
  inside the milestone that closes the plan. The one-commit-per-finding rule
  is not being weakened in the skill. 2026-09-17.
- Decision: `prose-duplication` ships as a sixth generic card in the payload,
  not as a live-only card and not as a prose rule.
  Rationale: the one-owner-per-fact check has now been run by hand in four
  consecutive milestones and found a real violation in three of them, which is
  exactly the "invariant re-enforced by a human twice" that
  `docs/capabilities/CARD_FORMAT.md` says is a check nobody wrote. It belongs
  in the payload rather than only here because the invariant is a property of
  any repository whose artifacts are prose, and because the sweep in
  `skills/doc-garden/SKILL.md` that currently does it by hand ships to every
  bootstrapped project. It carries no gating row in `docs/MATURITY.md`: the
  check finds candidate pairs but cannot decide which copy is canonical, so no
  human gate retires when it goes green. Not graduated as a decision record —
  the card is the artifact that owns it. 2026-09-17.
- Decision: The card's exclusion classes were derived from the sweep's own
  output, and two of them were discovered only because the first draft was
  run: format-governed siblings, and whole directories whose files restate by
  construction (`plans/` and `docs/decisions/`).
  Rationale: a first draft with four exclusion classes reported 29 pairs on
  this tree, of which 27 were the card format's own section scaffolding or
  byte-identical template ↔ live counterparts. A card that would have been
  built from that draft is the card that gets switched off in a week, which is
  the failure `CARD_FORMAT.md` names explicitly. Deriving the exclusions from
  a run is what makes the remaining two findings meaningful. 2026-09-17.
- Decision: `D3` is recorded as overdue with its evidence rather than paid
  down, and `0016` accepts the two irreducible skill ↔ card overlaps instead
  of resolving them.
  Rationale: paying `D3` means instantiating five cards, five register rows
  and the gating rows behind them — a plan of its own, and doing it inside the
  milestone that closes this plan is exactly the scope widening
  `skills/plan-execute/SKILL.md` forbids. The overlaps are accepted because
  the reference rules make a pointer impossible in both directions, and the
  alternatives were a new artifact created to satisfy a rule or a procedure
  that breaks in any project that pruned the card; `0016` records the
  reasoning because it will otherwise be re-litigated by the first agent that
  runs the check. 2026-09-17.

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
- Observation: The live fill check matched itself. Live artifacts that
  *discuss* the marker convention are indistinguishable, to a flat grep,
  from artifacts still carrying markers — and the command line published in
  `AGENTS.md` contains the pattern it searches for, so it always matched
  its own file.
  Evidence: the first run of
  `grep -rn '{{FILL\|GUIDANCE' AGENTS.md GOALS.md ARCHITECTURE.md docs/…`
  returned `AGENTS.md:41` (the command itself), `ARCHITECTURE.md:21` (prose
  naming the markers), and `docs/MATURITY.md:4` (a real unfilled skeleton).
  Two of three hits were false. Resolved by writing the pattern with
  bracketed final letters (`{{FIL[L]`, `GUIDANC[E]`) so it cannot match its
  own command line, and by deleting `ARCHITECTURE.md`'s restatement of the
  marker syntax, which `template/AGENTS.md` already owns — the false
  positive and a one-owner-per-fact violation turned out to be the same
  defect. The residual limitation is registered as `D2` in `docs/DEBT.md`.
  After both fixes the check reports 24 hits, all in `docs/MATURITY.md`.
- Observation: Naive heading comparison cannot test template ↔ live
  structural correspondence, because some headings are content. Four of
  eleven files "differed" on a plain heading diff, and all four differences
  were headings that are themselves filled slots — component names,
  principle names, a debt item title, and every file's own title.
  Evidence: `diff <(grep '^#' template/ARCHITECTURE.md) <(grep '^#'
  ARCHITECTURE.md)` reported `### {{FILL: component name}}` ×2 against
  `### Template payload`, `### Live instantiation`, `### Plan layer`,
  `### Skill layer`. The test that does work is a subsequence check — drop
  template headings containing a fill slot, require the remainder to appear
  in the live file in order — which passed for all 11 files (template
  heading counts 2–13, fixed 1–13, live 2–13, zero failures). The rule is
  recorded in `docs/DEBT.md` `D1` as what a mechanism must implement.
- Observation: The live repository was missing `plans/completed/`, which
  `AGENTS.md` had claimed since the seed commit. It was found by
  mechanically resolving every backticked path in the map rather than by
  reading the map.
  Evidence: resolving the map's 14 backticked paths reported `MISS
  plans/completed/` alongside two bare filenames (`DECISION_FORMAT.md`,
  `CARD_FORMAT.md`) that resolved only relative to their directory; the
  directory was created with a `.gitkeep`, the two map entries were
  rewritten as repository-relative paths, and the re-run reported
  `unresolved: []`.
- Observation: Reconciling the three seeded files against the template cost
  one command each and found exactly one nonconformant file.
  `plans/PLANS.md` was byte-identical, `GOALS.md`'s heading outline matched
  the template's exactly, and `AGENTS.md` was missing the `## Commands`
  section entirely plus two working rules.
  Evidence: `cmp template/plans/PLANS.md plans/PLANS.md` silent;
  `diff <(grep '^#' template/GOALS.md) <(grep '^#' GOALS.md)` empty;
  the same diff for `AGENTS.md` showed `## Commands` and `## Working rules`
  present in the template against `## Working rules` alone in the live file.
- Observation: Scanning every backticked path across the live artifacts
  found six that do not resolve from the repository root, and only one was
  a defect. The rest fall into three classes a link checker has to
  tolerate: sibling-relative names inside the directory that owns them
  (`CARD_FORMAT.md` cited from `docs/capabilities/index.md`), illustrative
  filenames in a format document (`NNNN-short-slug.md`,
  `0007-single-writer-per-queue.md`), and deliberate mentions of files that
  must not or do not yet exist (`PROMPTS.md` as a `GOALS.md` non-goal,
  `CLAUDE.md` as the shim M9 installs).
  Evidence: resolving the 27 distinct backticked paths across `AGENTS.md`,
  `GOALS.md`, `ARCHITECTURE.md`, `docs/PRINCIPLES.md`, `docs/DEBT.md`, the
  two index files, and the 13 decision records left the six above. The one
  defect was `skills/`, referenced as a real directory by three live files;
  it now exists with a `.gitkeep`, which also makes `ARCHITECTURE.md`'s
  declared-but-empty skill layer a true statement rather than a promise.
  This classification is input for M6's `doc-integrity` card: a checker
  that flags all six is a checker that gets switched off.
- Observation: The template still contains zero references outward, but the
  obvious grep for it over-reports. Searching `template/` for
  `plans/active` alongside the blueprint-specific terms returned two hits —
  `template/AGENTS.md`'s map line and a line of the adopted convention —
  both of which are template-provided paths that resolve inside any
  instantiation.
  Evidence: `grep -rniE 'harness-blueprint|blueprint|skills/|v1-blueprint|
  plans/active' template/ | wc -l` → `2`, both `plans/active/`; dropping
  that term from the pattern returns `0`. The rule a mechanism needs is
  "no path that fails to resolve after a plain copy", not "no path".
- Observation: The counterpart walk found a correspondence hole that
  predates this milestone and is not a defect: `template/plans/active/`
  ships a `.gitkeep`, while live `plans/active/` holds a real plan and needs
  none. Placeholder semantics are an exclusion the rule had not stated.
  Evidence: `for f in $(cd template && find . -type f); do [ -e "${f#./}" ]
  || echo "MISS ${f#./}"; done` → `MISS plans/active/.gitkeep`, the only
  miss across 13 template files.
- Observation: Resolving backticked paths across the eight new cards left
  nine unresolved references, none of them defects, and they add a class
  M5's classification did not have: paths that must *not* exist, because
  they appear inside failing-case instructions and remediation examples.
  Evidence: `docs/NOPE.md` ×3 in `doc-integrity`,
  `template/docs/EXAMPLE.md` and `docs/EXAMPLE.md` in
  `template-live-drift`, `skills/harness-init/README.md` in
  `blueprint-eval`; plus `NNNN-short-slug.md` and the suffix pattern
  `_FORMAT.md` (illustrative filenames), and `skills/harness-init/SKILL.md`,
  a forward reference to a file M9 creates. A checker cannot infer any of
  these, which is why `doc-integrity` specifies an allowlist of exact
  `path:reference` pairs.
- Observation: The live tree is fully filled for the first time — the marker
  check that reported 24 hits after M5 now reports none, so its published
  expectation in `AGENTS.md` changed from "`docs/MATURITY.md` is the only
  expected hit today" to "silence is a pass".
  Evidence: `grep -rn '{{FIL[L]\|GUIDANC[E]' AGENTS.md GOALS.md
  ARCHITECTURE.md docs/PRINCIPLES.md docs/MATURITY.md docs/DEBT.md
  docs/specs/index.md docs/capabilities/index.md` → exit 1, no output.
- Observation: The payload's own vocabulary collides with ordinary English
  in one place worth watching. A per-stack hint in `doc-integrity` said
  "documentation test harness", where "harness" means a test runner and not
  the agent harness the blueprint is about; it was reworded to "test suite".
  Evidence: `grep -rniE 'harness|claude|codex' template/docs/capabilities/`
  returned that single line, and returns nothing after the edit.
- Observation: The rule-duplication ban is only checkable mechanically, and
  the check caught a real violation immediately. Comparing six-, seven- and
  eight-word windows of each skill body against `plans/PLANS.md` reported
  ten, eight and six shared windows for `plan-author` — all one sentence,
  the convention's own statement about plans that outgrow one file, which
  the refusal list had reproduced verbatim while I believed I was
  paraphrasing.
  Evidence: the first run printed `n=6 overlaps: 10` including
  `'file is two plans the first'` and `'plans the first naming its
  successor'` for `plan-author` and `0` at every window size for
  `plan-execute`; after rewriting the bullet to state the refusal and point
  at the convention, both files report `0` at n=6, 7 and 8.
- Observation: The skill layer's dependency rule was too permissive as
  written, and writing the first skill is what exposed it. `ARCHITECTURE.md`
  allowed skills to name `docs/**`, and the procedure wanted to cite
  `docs/decisions/0013-convention-change-triggers-plan-audit.md` — a path
  that resolves in this repository and in no other, so the reference would
  have shipped broken to every bootstrapped project.
  Evidence: resolving the backticked paths in both skill bodies from the
  repository root reported `unresolved: []` across fourteen distinct paths
  (nine in `plan-author`, five in `plan-execute`) and zero mentions of
  `template/`, but only after the citation was replaced by a disclaimer;
  the rule now names directories and bans the project-specific files inside
  them.
- Observation: A skill body is a third size class in this repository. The
  two files are 136 and 111 lines — larger than any docs artifact except
  `MATURITY.md` — and the reason is that a procedure has to carry its steps,
  its refusals and its stop condition, none of which another file can own.
  The `AGENTS.md` context-tax cap does not apply, because a skill is read
  only by the session that invokes it, not at every session start.
  Evidence: `wc -l` over the two `SKILL.md` files against 111 for
  `ARCHITECTURE.md`, 69 for `AGENTS.md`, and 36 for `docs/PRINCIPLES.md`.
- Observation: This milestone was executed by following `plan-execute`'s own
  procedure, which is the only trial the skill has had, and one step of it
  failed on the first attempt: the cheap verification commands were run
  after the edits rather than before them, so the session has no observation
  of the pre-edit state to distinguish a failure it caused from one it
  inherited. Both checks pass now, and the gap is recorded rather than
  papered over — the skill says to observe the starting state first for
  exactly this reason.
  Evidence: `cmp template/plans/PLANS.md plans/PLANS.md` silent and the
  marker grep exiting 1 with no output, both observed after
  `AGENTS.md`/`ARCHITECTURE.md` were already edited; the correspondence walk
  over all 18 template files reported `fails: 0`, with seven excluded — the
  five generic card files and the two `.gitkeep` placeholders under
  `template/plans/` — and the eleven remaining pairs passing by subsequence
  or, for `plans/PLANS.md`, by byte identity.
- Observation: The skill layer's reference rule forbids what M9's milestone
  text requires. `ARCHITECTURE.md` states that a skill body may not name
  `template/`, and M9 asks `harness-init` to "copy and fill `template/`" —
  so the sixth skill is the one skill whose subject matter is the payload
  directory it may not name. The tension is real rather than verbal: every
  other skill runs inside a bootstrapped project, where `template/` is
  genuinely absent, while `harness-init` runs against a payload it must
  locate. M9 has to settle it, and the two shapes available are an exemption
  stated in `ARCHITECTURE.md` (this one skill names the payload because it
  is the only one that executes outside a bootstrapped project) or a
  parameterized phrasing that never writes the path (the payload directory
  of the checkout the skill was invoked from). This milestone did not choose;
  it recorded the conflict on discovery.
  Evidence: `ARCHITECTURE.md`'s layer map — "Skill bodies may not name
  `template/`, because a target project does not receive it" — against the
  M9 milestone text below; the three skills written here needed no such
  reference and report zero `template/` mentions.
- Observation: The non-duplication shingle check came up clean on the first
  run for all three skills, and instead caught an overlap between two new
  skills — a class M7's version of the check never looked for. `doc-garden`
  and `retro` had independently written the same eight-word window while
  telling their reader to consult the capability register, because both
  consult it for nearly the same reason.
  Evidence: against `plans/PLANS.md`, `n=6/7/8 overlaps=0` for all three
  bodies on the first run; the pairwise scan at `n=8` printed one shared
  window, `'docs capabilities index md which invariants are already'`,
  between `doc-garden` and `retro`; after rewriting `retro`'s line the
  pairwise scan reports `0` for every pair across all five skills.
- Observation: Only one live artifact claimed anything about which skills
  exist, and finding that out cost one grep rather than a reading pass. Two
  other mentions of the missing procedures are not state claims and were
  correctly left alone.
  Evidence: `grep -rn "doc-garden\|capability-build\|retro\b\|harness-init"
  AGENTS.md GOALS.md ARCHITECTURE.md docs/` returned `ARCHITECTURE.md:47`
  (the Skill layer entry, edited), `GOALS.md:25–26` and `:84` (the roster as
  an intended outcome, not a present-tense claim), `docs/capabilities/
  blueprint-eval.md:50–62` (a failing-case instruction that deliberately
  names a file which must not exist), and
  `docs/decisions/0009`/`0013` (an immutable roster decision and the record
  naming `doc-garden` as the audit's owner, now satisfied).
- Observation: A skill's procedure shape is set by whether its steps are
  ordered, and one of the three turned out to be. `capability-build` carries
  a numbered sequence because its steps are a proof whose order is load
  bearing; `doc-garden`'s sweeps are a set that can be run in any order, and
  `retro`'s phases are sequential but coarse. The five skills now span 111
  to 150 lines with no correlation to their procedure's shape.
  Evidence: `grep -c ''` over the five bodies → `capability-build` 126,
  `doc-garden` 145, `plan-author` 136, `plan-execute` 111, `retro` 150.
- Observation: The portability check set came up clean on the first run for
  the sixth skill — including the two checks most at risk from its subject
  matter, since a procedure about installing a payload wrote zero mentions
  of `template/` and a procedure about a harness entry point wrote zero
  harness vocabulary. The parameterized phrasing is what made both possible.
  Evidence: frontmatter keys `name,description` with `name` equal to the
  directory for all six; `Never` and `Stop condition` present in all six;
  fourteen distinct backticked paths in `harness-init`, `unresolved: []`
  against the repository root, and every non-skill path among them also
  present under `template/`; `template/` mentions `0`, blueprint mentions
  `0`, harness-vocabulary matches `[]`; shingle overlap against
  `plans/PLANS.md` `0` at n=6, 7 and 8, and `0` for all 15 sibling pairs at
  n=8.
- Observation: The verification step of a milestone about portability found
  that the thing being made portable is not discoverable. Every harness that
  loads procedures automatically reads them from a root it picks itself; a
  repository-root `skills/` is not one of those roots, and reaching it takes
  configuration. The blueprint's layout claim is therefore half true: the
  file shape is the intersection of the conventions, the location is not.
  Evidence: the documented discovery layout is one level under a skills root
  (`<skills-root>/<skill-name>/SKILL.md`) with `name` defaulting to the
  directory and `description` required, which all six files satisfy; but the
  project-level scan is `<ancestor>/.omp/skills/*/SKILL.md` plus
  `~/.omp/agent/skills/*/SKILL.md`, with anything else reached only through
  a configured extra directory. The corresponding roots for the other two
  harnesses are not stated in the documentation reachable from this session,
  so this observation is one harness observed and two inferred — recorded as
  `D4`, whose trigger is the live trial rather than another reading.
- Observation: A misaimed edit inside the new skill deleted two lines of one
  step while inserting text meant for another, and the repair was visible
  only because the surrounding step stopped parsing as prose. Worth
  recording as the failure mode it is: an edit anchored by remembered line
  numbers after the file had already been rewritten once in the same
  session.
  Evidence: step 5's opening two lines were replaced by step 2's new
  sentence, leaving "merging it is a decision, not a step." followed by
  "somebody will otherwise propose them"; both steps were restored from the
  written content and the file now reads 182 lines, the largest of the six.
- Observation: The bootstrap flow's references survive a real copy, including
  the procedures, which is the strongest thing paper verification could
  establish and was cheaper than reading.
  Evidence: `T=$(mktemp -d); cp -R template/. "$T/"; cp -R skills "$T/skills"`
  produced 24 files; resolving every backticked candidate path inside the copy
  left eight unresolved strings, all in the three legitimate classes —
  `/tmp/<project>.sock`, `0007-single-writer-per-queue.md`,
  `NNNN-short-slug.md`, `_FORMAT.md`, `docs/capabilities/<card-name>.md`,
  `CARD_FORMAT.md`, `index.md`, `evidence-check.md`. Zero unresolved paths
  came from the six procedure bodies. The payload's own step-10 checks also
  pass out of the box: six card files, six register rows, five gating rows
  each naming a card that exists.
- Observation: The paper walk found exactly one overclaim, and it was a claim
  about the payload rather than about the flow. `harness-init` told its reader
  that each skeleton's guidance states how to shrink that file for a small
  project; one of eight does.
  Evidence: extracting the `GUIDANCE —` headings from all eight skeletons
  returned `RIGHT-SIZING` only in `template/ARCHITECTURE.md`; the other seven
  carry owns / must-not-absorb / anti-pattern and nothing about shrinking. The
  sentence was corrected to say what is there, and to point at the skill's own
  right-sizing section, which is where the rule actually lives.
- Observation: The reference sweep found one real defect, and it was in the
  one file class this repository had decided never to edit. Decision record
  `0015` cited `MATURITY.md`, which resolves from neither the repository root
  nor `docs/decisions/`.
  Evidence: resolving the backticked paths across 37 live markdown files left
  eleven unresolved strings, ten of them the known legitimate classes and one
  a genuine miss. The repair forced the append-only question: the rule's point
  is that reasoning is never rewritten, and a path repair changes no
  reasoning, so `DECISION_FORMAT.md` now says so in both copies rather than
  leaving the next agent to guess whether a dangling citation is fixable.
- Observation: One-owner-per-fact was being violated eight times in the live
  tree, and every violation was a pointer that had grown a copy of what it
  pointed at — most of them in `AGENTS.md`, the file whose whole job is
  pointing.
  Evidence: intersecting eight-word windows over the live artifacts and
  skills reported twelve pairs before the fixes and four after: `AGENTS.md`
  had restated `GOALS.md`'s purpose sentence (13 windows), `ARCHITECTURE.md`'s
  layer rule (16), and `D1`'s mechanism sentence (7); `GOALS.md` had restated
  the convention's durable-context sentence (6); `docs/DEBT.md` had restated
  the correspondence invariant it says it is not a copy of (8 against
  `ARCHITECTURE.md`, 10 against the card); `skills/capability-build/SKILL.md`
  had restated two rules from `CARD_FORMAT.md` (4). The four that remain are
  an attributed restatement (×2), the two format documents' shared
  boilerplate, and the artifact path list that `AGENTS.md` and
  `ARCHITECTURE.md` must both spell out.
- Observation: A duplication check written from the principle alone would have
  been unusable, and only running it showed why. With the whole tree in scope
  it reports the card format's section scaffolding on every card pair and 488
  windows between two files a rule requires to be byte-identical.
  Evidence: the first draft's four exclusion classes left 29 pairs on this
  tree; adding same-format siblings, declared-correspondence counterparts, and
  the two restate-by-construction directories (`plans/`, `docs/decisions/`)
  left 3 of 435 pairs over 30 files and 21,890 windows. Two are the skill ↔
  card pairs `0016` accepts (11 windows); the third is the six procedure names
  listed in both `ARCHITECTURE.md` and `docs/specs/bootstrap-flow.md`, which
  is the allowlist class the card describes.
- Observation: `D3`'s trigger had already fired and nobody noticed, which is
  the thing debt rows are supposed to prevent. Both defects this milestone
  found are decided by cards the payload ships and this project chose not to
  instantiate.
  Evidence: the dangling reference is `doc-integrity`'s invariant; the eight
  duplicated pairs are `prose-duplication`'s. Both had been in the tree for at
  least one milestone. `D1`'s trigger was also mis-stated — as written it
  fires on any one-sided commit, including the project-content edits that make
  up most of the live half's history — and was sharpened to fire on a generic
  change landing in one half only.

## Outcomes & Retrospective

### Purpose against outcome

The purpose was that an agent in any of three harnesses could be pointed at a
new project, invoke a bootstrap procedure, and end up with a right-sized
working harness — and that this would be visible by copying the payload into
an empty directory, following a procedure by hand, and finding that every
referenced artifact exists and every rule is executable from repository
content alone. That is what closing the plan actually did, with a real copy
rather than an imagined one: 24 files, every backticked path inside the copy
resolving except the eight strings that fall in the three legitimate classes,
and zero unresolved paths in the six procedure bodies. The payload also passes
the self-consistency checks its own bootstrap procedure prescribes.

What the plan delivered beyond its purpose is the thing it set out to be: this
repository is client #1, filled from its own payload, and every milestone from
M5 onward was executed by the procedures being written. M7 through M10 each
found a real defect in the artifact set by running a check rather than by
reading it, which is the entire thesis of the constraint layer arriving as
specifications early.

What it did not deliver is the one thing `GOALS.md` already listed as a known
unknown: nobody has bootstrapped a different project from this payload, and no
feature has been driven through the loop anywhere but here. The three-harness
claim is verified as file shape and reference resolution, not as behavior, and
M9 falsified the stronger reading of it — the procedures sit outside every
harness's auto-discovery root. Both gaps are recorded where gaps get picked up
cold: `D4` and `D7` in `docs/DEBT.md`, with the trial specified as
`docs/capabilities/blueprint-eval.md`.

### What remains open

`docs/DEBT.md` carries seven rows. `D1`, `D2` and `D4` were opened by earlier
milestones; `D3` is now overdue rather than deferred, because both defects
this milestone found are decided by cards the payload ships and this project
chose not to instantiate; `D5`, `D6` and `D7` were opened here for the
unattended loop, the brownfield retrofit, and the live trial. Nothing in the
register is a surprise to its own card: each row points at the specification
and states only why the work is not being done and what ends the deferral.

The first thing a successor plan should do is pay `D3` down, because it is the
cheapest of the seven and it retires the two hand checks this session had to
run twice.

### Lessons routed, one owner each

Four differences passed all three materiality questions.

A restatement that cannot be avoided still needs an owner named in it. Routed
to `docs/PRINCIPLES.md`, which now says that a forced restatement — a card
stating the invariant it checks, a format document the shape it defines —
names the owning file in the same section, and that nothing else earns one.
This is what turned three of the four surviving duplication pairs from
findings into legitimate cases.

Append-only was blocking a repair it was never about. Routed to
`docs/decisions/DECISION_FORMAT.md` in both copies: the rule governs a
record's reasoning, and a reference inside a record that has stopped resolving
is repaired in place because a citation nobody can follow protects nothing.

One-owner-per-fact was being enforced by a script written from scratch in four
consecutive sessions. Routed to a sixth generic card,
`template/docs/capabilities/prose-duplication.md`, specced and unbuilt, whose
exclusion classes are the ones the runs actually produced rather than the ones
the principle suggests. It gates no rung, because the check finds candidate
pairs and cannot decide which copy is canonical.

Two overlaps are structurally irreducible: a portable procedure and a prunable
card cannot name each other, so both state the shared fact. Routed to
`docs/decisions/0016-skill-and-card-may-restate-one-fact.md`, which accepts
them with measurements and states what the acceptance does not license.

### No change, with the question that failed

A session ran the cheap verification commands after its edits instead of
before them (M7). No change: `skills/plan-execute/SKILL.md` already says to
observe the starting state first. Fails question two — an edit that restates a
rule already written costs every future reader and saves nothing.

An edit anchored on remembered line numbers damaged two steps of a file that
had already been rewritten in the same session (M9). No change: the only
transferable statement is "re-read before editing", and no artifact in this
set owns the mechanics of editing. Fails question three — as a line in a
procedure it would be advice rather than something a reader applies to a
concrete case, and creating an artifact to hold it is forbidden.

Four of the ten completed milestones carried a named carve-out or substitution
against their own written acceptance (M3, M5, M8, M10). No change: every one
was stated against the acceptance it diverged from, in the session that caused
it, which is exactly what the convention and
`skills/plan-execute/SKILL.md` require. Fails question one — the divergence
that would be worth a rule is an unnamed one, and there were none.

M8 was authored with an option to split into two sessions and did not need it;
every milestone fit one session. No change, question one: a milestone that fit
is not a divergence to learn from.

### What this pass could not decide

Whether the payload is right-sized for a project that is not this one. Every
judgement about that came from reading the artifacts rather than from watching
somebody use them, and the two candidate failures are opposite: an artifact
set too heavy for a small project, and a starter card set too thin for a real
codebase. `blueprint-eval` is written to answer it; until it runs, the
question stays open rather than answered optimistically.

## Context & Orientation

This repository holds its own live instantiation, and every milestone of this
plan is complete. `GOALS.md` (project boundaries — read it first),
`AGENTS.md` (entry-point map, with the two cheap verification commands under
Commands), `ARCHITECTURE.md` (components and the layer map), `docs/`
(`PRINCIPLES.md`, the filled `MATURITY.md`, `DEBT.md` with seven register
rows, `decisions/0001`–`0016` plus `DECISION_FORMAT.md`, `specs/index.md`
with `specs/bootstrap-flow.md` as the first reflected spec,
`capabilities/index.md` plus `CARD_FORMAT.md` and this project's three
cards), `plans/PLANS.md`, the `template/` payload with its six generic cards,
`skills/` holding all six procedures — `harness-init`, `plan-author`,
`plan-execute`, `doc-garden`, `retro` and `capability-build` — and the root
`CLAUDE.md` shim. Nothing below remains to be created; this file moves to
`plans/completed/` as its last act.

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
  same-relative-path live counterpart at repo root; structure corresponds by
  subsequence, only project-specific content varies. Two exclusions (M6):
  card files under `template/docs/capabilities/`, which are project content
  a target project prunes, and `.gitkeep` placeholders. The two index files
  and both format documents are not excluded.
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
  subsume`. M6 filled the rung definitions, promotion rule, demotion rule
  and gating table in both copies and left exactly one slot in the template
  copy — Current rung — so the file stays in the skeleton class. Template
  gating rows: `fast-verify` and `evidence-check` at L1, `doc-integrity`,
  `boundary-lint` and `isolated-env` at L2, no L3 row. Live gating rows:
  `template-live-drift` at L1, `blueprint-eval` and `loop-runner` at L2.
- **DEBT.md shape** (M4): a Register table with columns
  `ID | Item | Where | Why deferred | Trigger to pay it down`, IDs of the
  form `D<n>` never reused, plus an optional per-item Details section for
  anything not pick-up-able from its row. Closing an item deletes its row
  and section. M10's deferred-work entries use this shape.
- **Structural correspondence test** (M5): template ↔ live structure is
  compared by subsequence, not by diff. Take the template file's headings,
  drop every heading containing a fill slot, and require the remainder to
  appear in the live counterpart in the same order; extra live headings are
  expected, since filled content adds them. Plain heading diff produces
  false drift on any file whose headings are content. M6 edits both copies
  of `MATURITY.md` and both `capabilities/index.md` files and must keep this
  test passing; the rule is also recorded in `docs/DEBT.md` `D1`.
- **Live verification commands** (M5): the two cheap checks are published
  under Commands in `AGENTS.md` — `cmp template/plans/PLANS.md
  plans/PLANS.md`, and a marker grep over exactly the eight live
  skeleton-derived files. The grep's pattern is written with bracketed
  final letters so it does not match its own command line, and its file set
  deliberately excludes `docs/decisions/` and the two format documents,
  which may quote markers freely. Any milestone adding a live
  skeleton-derived artifact adds it to that list; M6 added cards, which
  carry no markers, so the set is unchanged and both checks are now silent
  on a clean tree.
- **Next free identifiers** (M9, supersedes the M6 count): decision records
  run `0001`–`0015`, so the next is `0016` and numbers are never reused; M9
  graduated none. Debt items run `D1`–`D4`, so the next is `D5`. M10's
  deferred-work entries continue that sequence.
- **Live-only files** (M5): correspondence runs template → live only.
  `docs/decisions/NNNN-*.md` and `plans/active/*` are project content with
  no template counterpart, and that is not drift; the reverse check does
  not exist.
- **Card set** (M6): the payload ships five cards at
  `template/docs/capabilities/<name>.md` — `fast-verify`, `evidence-check`,
  `doc-integrity`, `boundary-lint`, `isolated-env` — and this project owns
  three at `docs/capabilities/<name>.md` — `template-live-drift`,
  `blueprint-eval`, `loop-runner`. All eight are `specced`; none is built.
  Card files carry no fill slots and no guidance blocks, which makes them a
  third form alongside the two template file classes: shipped verbatim like
  the format documents, but prunable like project content. M9's
  `harness-init` right-sizes by deleting a card file together with its
  register row and its gating row, and records the omission in `GOALS.md`
  under scope.
- **Card scope rule** (M6): one decidable invariant per card, and anything
  adjacent that a checker cannot decide is named in the card as out of scope
  with its owner. M8's `capability-build` inherits this: a card whose
  Invariant is a capability rather than a property has no acceptance that
  can gate promotion, and is a card to rewrite before building.
- **Skill body shape** (M7): a `SKILL.md` body carries five kinds of section
  and nothing else — when to invoke (including when not to), what to read
  before acting, the numbered or narrated procedure, an explicit `Never`
  list, and a `Stop condition`. The refusal list and the stop condition are
  the part a skill owns outright: the convention says what a plan must be,
  and the skill says what the agent running the procedure must not do. Rules
  that belong to an artifact are referenced by path, never restated. M8's
  three skills and M9's `harness-init` conform to this shape.
- **Skill non-duplication check** (M7): the ban on copying rule text out of
  `plans/PLANS.md` is verified mechanically, by intersecting the word
  n-gram windows of a skill body with those of the convention. Both M7
  skills report zero shared windows at n = 6, 7 and 8 after one fix. The
  check is twenty lines of throwaway script — normalize to lowercase words,
  build the shingle sets, intersect — and finding a real violation on its
  first run is the reason M8 runs it too rather than trusting a reading.
- **Skill reference scope** (M7, amends the M5 layer map): a skill may name
  payload-provided artifacts and the `docs/` and `plans/` directories, but
  not a project-specific file inside them — no decision record, spec, card,
  or plan file — because such a path resolves only in the project that wrote
  it. `ARCHITECTURE.md` now states this. Two consequences for M9: the
  artifacts skills name are the part of the payload right-sizing may not
  prune, or else `harness-init` must rewrite the referring skill when it
  prunes one, and the right-sizing rules have to say which.
- **Cross-skill handoffs** (M7): `plan-author` and `plan-execute` each name
  the other by path as the boundary of their own authority — authoring hands
  off to execution, execution hands a defective plan back to authoring.
  `plan-author` additionally describes how a convention-change audit is
  recorded while explicitly disclaiming ownership of its trigger, so M8's
  `doc-garden` must state that trigger; if it does not, the rule that
  `docs/decisions/0013-convention-change-triggers-plan-audit.md` graduated
  has no procedure carrying it.
- **Skill set state** (M8): five skills exist — `plan-author`,
  `plan-execute`, `doc-garden`, `retro`, `capability-build` — all conforming
  to the M7 body shape, all reporting zero shared six-, seven- and
  eight-word windows against `plans/PLANS.md` and zero eight-word windows
  against each other. `harness-init` is the sixth and last; M9's phrase
  "install the other five skills" refers to exactly this set.
- **Handoff graph is closed** (M8, extends the M7 contract): `doc-garden`
  hands a swept gap that wants a mechanism to `capability-build` and a
  multi-file repair — plus the question of where an audit outcome is
  recorded — to `plan-author`; `retro` routes a mechanizable lesson to
  `capability-build` and states that a `plans/PLANS.md` edit obliges the
  audit `doc-garden` owns; `plan-author` and `plan-execute` name each other.
  Every skill is therefore named by at least one sibling, which settles the
  M7 question about right-sizing: `harness-init` may not prune a skill,
  because pruning any one of the five dangles a reference in another. The
  right-sizing rules M9 writes apply to payload artifacts and cards, not to
  the skill set.
- **Skill portability check set** (M8): the checks each skill-writing
  milestone runs, all of which pass on the five existing skills — frontmatter
  keys exactly `name` and `description` with `name` equal to the directory;
  a `Never` section and a `Stop condition` section present; every backticked
  repository-relative path resolving from the repository root; zero mentions
  of `template/`, of this repository, and of harness-specific vocabulary
  (matched as `claude|codex|omp|cursor|copilot|mcp|subagent|slash command|
  read tool|bash tool|grep tool|tool call|prompt`); zero shingle overlap
  against the convention at n = 6, 7, 8 and against sibling skills at n = 8.
  M9 runs the same set on `harness-init`, with the `template/` clause
  pending the exemption question recorded in Surprises.
- **Project-specific invariants inside portable skills** (M8): a skill that
  must sweep, check, or reason about an invariant only one project declares
  names the artifact that declares it rather than the invariant's subject.
  `doc-garden`'s correspondence sweep reads `ARCHITECTURE.md` and walks
  whatever structural correspondence it finds declared there, which is how
  this repository's template ↔ live rules get swept by a body that cannot
  name `template/`. M9 and M10 reuse this shape wherever a procedure needs a
  fact that lives in a project's own artifacts.
- **Skill set complete** (M9): six procedures exist — `harness-init`,
  `plan-author`, `plan-execute`, `doc-garden`, `retro`, `capability-build`.
  All six pass the M8 portability check set, including the `template/` and
  harness-vocabulary clauses, with no exemption anywhere; `ARCHITECTURE.md`
  names six and states how the bootstrap procedure resolves the payload
  directory and the native entry-point filename at run time instead of
  naming either. Nothing further may be added to the roster in v1:
  `docs/decisions/0009` fixes it at six.
- **Right-sizing rules** (M9, settles the M7 question): stated in
  `skills/harness-init/SKILL.md` under its own heading. Structure never
  shrinks, because every mapped artifact is named by some procedure; what
  shrinks is the starter card set, and omitting a card deletes three things
  together — the card file, its register row, and any gating row in
  `docs/MATURITY.md`. Empty headings inside a kept file are deleted rather
  than left open. An artifact omitted by the owners' choice also loses its
  map line, and the omission plus the procedure reference it breaks is
  recorded in `GOALS.md` under scope. The procedures themselves are never
  pruned.
- **Harness entry point** (M9): root `CLAUDE.md` holds exactly `@AGENTS.md`
  and eleven bytes; it is live-only, with no counterpart in the payload,
  because which filename a harness reads natively is a property of the
  environment rather than of the project. `AGENTS.md`'s map carries its
  one-line entry. M10's reference sweep should expect `CLAUDE.md` to
  resolve now, where M5's classification listed it as a deliberate mention
  of a file that did not yet exist.
- **Skill discovery is not automatic** (M9): the six procedures conform to
  the universal skill layout — one directory each, one `SKILL.md`, `name`
  equal to the directory, a one-line `description` — but a repository-root
  `skills/` is not an auto-discovery root for the harnesses, which read from
  roots of their own. Reading a procedure by its mapped path is the whole
  mechanism today, and it works everywhere. Registered as `D4` in
  `docs/DEBT.md` with the live trial as its trigger; `harness-init` step 9
  tells a bootstrapping agent to bridge it in the environment it is in.
- **Card set, final** (M10, supersedes the M6 count): the payload ships six
  cards at `template/docs/capabilities/<name>.md` — `fast-verify`,
  `evidence-check`, `doc-integrity`, `boundary-lint`, `isolated-env`,
  `prose-duplication` — and this project owns three at
  `docs/capabilities/<name>.md` — `template-live-drift`, `blueprint-eval`,
  `loop-runner`. All nine are `specced`; none is built. `prose-duplication`
  has a register row in the payload and no gating row in either
  `docs/MATURITY.md`, because a check that reports candidate pairs without
  deciding which copy is canonical retires no human gate.
- **Duplication check, as run by hand** (M10): normalize each artifact to
  lowercase word sequences with indented and fenced blocks stripped, take
  eight-word windows, intersect pairwise. Exclusions, all observed to be
  necessary on this tree: `plans/` and `docs/decisions/` out of the checked
  set; pairs a declared correspondence requires to be copies; pairs whose
  shape one format document defines; the two format documents against each
  other; attributed restatements. `template/docs/capabilities/prose-duplication.md`
  is the specification; a later session should run the card, not re-derive the
  script.
- **Next free identifiers** (M10, supersedes M9): decision records run
  `0001`–`0016`, so the next is `0017`. Debt items run `D1`–`D7`, so the next
  is `D8`. `D3` is overdue rather than deferred.
- **Specs state** (M10): `docs/specs/bootstrap-flow.md` is the first
  reflected spec and `docs/specs/index.md` is now a one-row table. The
  reflection rule is satisfied for this plan: what an outside reader can now
  observe — a payload plus six procedures that install by copying, and which
  of the claims about them have been checked — is written there in the present
  tense, and this plan is the archive of how it came to exist.

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
- 2026-09-17: M5 executed. Added four Interfaces & Dependencies contracts
  (structural correspondence by subsequence, the two published live
  verification commands and their file set, next free decision and debt
  identifiers, live-only files), five Decision Log entries, and four
  Surprises observations. Reason: M6 through M10 edit both copies of files
  this milestone created and must keep the correspondence and fill checks
  passing, and neither check is discoverable from the artifacts alone — the
  subsequence rule in particular is the difference between a working drift
  test and one that reports false drift on four of eleven files. No
  milestone scope changed. M5's written acceptance is met with one named
  carve-out: live `docs/MATURITY.md` is an unfilled skeleton copy because
  M6 owns its content in both copies, so "every template skeleton has a
  live counterpart" holds structurally while that one counterpart is not
  yet filled.
- 2026-09-17: Post-M5 review — corrected ARCHITECTURE.md correspondence
  invariant to the subsequence form; it overstated the rule its own D1 and
  the plan contract define. Reason: an invariant falsified by the current
  tree is worse than none.
- 2026-09-17: M6 executed. Added two Interfaces & Dependencies contracts
  (card set, card scope rule), amended three (correspondence exclusions,
  MATURITY.md fill state and gating rows, next free identifiers), added four
  Decision Log entries and four Surprises observations, and graduated
  `0014`–`0015`. Reason: M7 through M10 read the card set and the gating
  rows, M8's `capability-build` needs the one-decidable-invariant rule, and
  M9's `harness-init` needs the prune-by-deletion behavior of cards — none
  of which is recoverable from the artifacts alone. Two amendments were
  corrections rather than additions: the correspondence contract was stated
  without exclusions that the tree now requires, and the MATURITY.md
  contract said M6 "replaces the slots", which would have put a project's
  rung into the payload.
- 2026-09-17: M7 executed. Added four Interfaces & Dependencies contracts
  (skill body shape, the non-duplication shingle check, skill reference
  scope, cross-skill handoffs), four Decision Log entries and four Surprises
  observations, and refreshed the Context & Orientation paragraph, which had
  gone stale at M6 by still claiming no card and no `MATURITY.md` content
  existed. Reason: M8 writes three more skills and M9 a sixth, all against a
  shape and a reference rule this milestone discovered rather than inherited
  — the layer map's `docs/**` permission turned out to allow a citation that
  dangles in every target project, and the duplication ban turned out to be
  uncheckable by reading. No milestone scope changed; no decision graduated,
  because the sharpened reference rule is now content in `ARCHITECTURE.md`
  and a second copy in `docs/decisions/` would be the drift this repository
  keeps legislating against.
- 2026-09-17: M8 executed. Added four Interfaces & Dependencies contracts
  (skill set state, the now-closed handoff graph, the skill portability
  check set, project-specific invariants inside portable skills), four
  Decision Log entries, four Surprises observations, and refreshed Context &
  Orientation. Reason: M9 writes the last skill and needs the check set
  stated as commands rather than as a remembered practice, and it needs the
  handoff graph, which answers M7's open right-sizing question in the
  direction M7 did not anticipate — no skill may be pruned, because every
  one is now named by a sibling. The milestone also surfaced a conflict M9
  must settle rather than discover: the layer rule banning `template/` from
  skill bodies collides with M9's instruction that `harness-init` copy the
  payload, and the two available shapes are recorded in Surprises. No
  milestone scope changed. M8's written acceptance is met with one named
  substitution: its template ↔ live sweep for `doc-garden` is stated
  generically, as a sweep of the structural correspondence
  `ARCHITECTURE.md` declares, because a skill body may not name the payload
  directory. No decision graduated — each entry either binds only the
  sessions writing skills or is already content in the artifact that owns
  it.
- 2026-09-17: Post-M8 review — reworded doc-garden sweep 2 (marker
  definition attributed to AGENTS.md, false post-fill in any bootstrapped
  project) and logged the skill-vs-card conceptual duplication for M10
  retro routing. Reason: portable skill claims must hold outside this
  repository.
- 2026-09-17: M9 executed. Added four Interfaces & Dependencies contracts
  (skill set complete, the right-sizing rules, the harness entry point,
  skill discovery is not automatic), superseded the identifier count with
  `D4` spent, added three Decision Log entries and three Surprises
  observations, and refreshed Context & Orientation. Reason: M10 walks the
  bootstrap flow on paper against exactly these rules, and two of them
  answer questions earlier milestones left open — the payload-naming
  conflict M8 recorded is settled by parameterization rather than by an
  exemption, and the right-sizing scope M7 asked about is settled as "cards
  only". The third is new: verifying the layout against harness discovery
  conventions showed that a repository-root `skills/` is not an
  auto-discovery root anywhere, which is registered as `D4` rather than
  fixed, because the fix is a payload change and the evidence for choosing
  one belongs to the live trial. M9's written acceptance is met without
  substitution; no milestone scope changed and no decision graduated.
- 2026-09-17: M10 executed and the plan closed. Added four Interfaces &
  Dependencies contracts (final card set, the duplication check as run,
  next free identifiers, specs state), five Decision Log entries, six
  Surprises observations, the full Outcomes & Retrospective, and a refreshed
  Context & Orientation; graduated `0016`. Reason: this is the last revision,
  so the file has to read as an archive rather than as a plan in flight —
  which means the retrospective states purpose against outcome, every routed
  lesson names its single owner, and every discarded one names the
  materiality question it failed, so a successor plan does not re-derive
  them. Two findings changed artifacts outside this plan's own text: the
  duplication sweep removed eight single-owner violations from the live
  artifacts, and the reference sweep repaired a dangling citation inside a
  decision record, which forced `DECISION_FORMAT.md` in both copies to say
  what append-only does and does not govern. M10's acceptance is met with one
  named deviation, logged above: the sweep's fixes landed as one commit
  rather than one per finding.
