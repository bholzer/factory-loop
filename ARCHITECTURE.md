# Architecture

## Shape

This repository is a document system, not a program: nothing here compiles,
and no artifact assumes a language toolchain. It has two halves that must not
be confused. `template/` is the payload — the artifact set a target project
receives by plain recursive copy, filled in on arrival. The repository root is
the live instantiation of that same payload for this project, which makes this
repo client #1 of its own bootstrap flow and makes divergence between the two
halves a bug in one of them. The organizing idea is direction of dependency:
the payload knows nothing about this repository, and everything here knows
about the payload.

## Components

### Template payload

- Lives in: `template/`
- Owns: the artifacts a target project starts from — skeletons carrying fill
  slots and authoring-guidance comments, format documents kept verbatim, the
  plan convention, and the starter set of capability cards a project prunes
  to fit. The marker syntax itself is defined in `template/AGENTS.md`.
- Does not: name this repository, or any path outside `template/`.

### Live instantiation

- Lives in: repository root — `AGENTS.md`, `GOALS.md`, `ARCHITECTURE.md`,
  `docs/`.
- Owns: this project's own knowledge layer, and the demonstration that the
  payload's skeletons are fillable into coherent artifacts.
- Does not: act as the payload's source of truth. A generic improvement made
  here is incomplete until `template/` carries it too.

### Plan layer

- Lives in: `plans/` — the convention in `plans/PLANS.md`, work in flight in
  `plans/active/`, finished work in `plans/completed/`.
- Owns: every unit of multi-file or multi-session work, and the observed
  evidence that it happened.
- Does not: describe how the system currently behaves (`docs/specs/`) or hold
  durable rationale after completion (`docs/decisions/`).

### Skill layer

- Lives in: `skills/<name>/SKILL.md`. Six exist — `harness-init`,
  `plan-author`, `plan-execute`, `doc-garden`, `retro`, and
  `capability-build`.
- Owns: the reusable procedures an agent invokes — bootstrap, plan authoring,
  plan execution, capability building, doc gardening, retrospection.
- Does not: carry rules that belong to an artifact. A skill references
  `plans/PLANS.md` and the docs tree; it never restates their content.

### Check layer

- Lives in: `tools/` — the aggregator `tools/verify`, one executable per
  check under `tools/checks/`, allowlists under `tools/allow/`,
  `tools/hooks/pre-commit`, the hook this repository keeps in version
  control rather than in each clone, and two drivers. `tools/blueprint-eval`
  builds a trial repository from both halves outside this tree and decides
  the trial's scriptable parts; `tools/loop-runner` advances a plan that
  lives in a checkout this repository does not own, one session per
  milestone, halting on the first session that left no advance behind.
  Neither is a check, both sit beside the aggregator rather than under
  `tools/checks/`, and the cheap command runs neither. The trial driver's
  subject is a filled copy of the payload, which exists only while a trial
  runs and never in a commit here; the loop driver's is an agent session in
  a foreign repository, which lasts as long as that session lasts. What does
  fit the budget is a self-test that drives the loop over fixture
  repositories, and `tools/checks/loop-runner` is it.
- Owns: the mechanical decisions this repository makes about itself. One
  check per card in `docs/capabilities/`, plus `scaffolding-markers`, whose
  invariant belongs to the marker definition rather than to a card.
- Does not: get copied into `template/`, and does not state an invariant. A
  shell script that assumes this file set is not portable payload, and the
  property a check decides is the card's to write down.

## Layer map and dependency rules

Two of the four components are portable payload — `template/` and `skills/` —
and portability is exactly a dependency restriction, so the layer map is
stated as rules a reader can decide by reading one line of text.

Nothing under `template/` may name a path outside `template/`, and nothing
there may mention this repository, the blueprint, or `skills/`. A target
project receives that directory's contents with no ancestor context, so an
outward reference is a dangling reference by construction.

Nothing under `skills/` may name a path that the payload does not provide.
The allowed targets are the artifacts a bootstrapped project actually
receives, which is decided by resolving the reference inside `template/`
rather than against a list anyone maintains by hand: `AGENTS.md`, `GOALS.md`,
`ARCHITECTURE.md`, `docs/PRINCIPLES.md`, `docs/MATURITY.md`, `docs/DEBT.md`,
the three `docs/` directories with their format documents and the two index
files the payload ships — `docs/capabilities/index.md` and
`docs/specs/index.md` — `plans/PLANS.md`, `plans/active/`,
`plans/completed/`, plus the procedure set itself, `skills/` and sibling
skills by `skills/<name>/SKILL.md`. Directories are namable; the
project-specific files inside them are not, because a decision record, a
spec, a card, or a plan file exists only in the project that wrote it.
Skill bodies may not name `template/`, because a target project does not
receive it, and may not name a harness-specific tool, because the same file
must work in every harness.

That rule binds the bootstrap procedure too, which is the only skill that
executes outside a bootstrapped project and whose subject matter is the
payload it may not name. It resolves both unnamable things at run time
instead: the payload directory through the `AGENTS.md` map of the checkout it
was invoked from, and the entry-point filename a harness reads natively
through the harness it is running in. No exemption exists, because a body
that named either one would ship a path that resolves in exactly one
checkout.

The live root artifacts may name `template/` freely: the payload is this
project's subject matter. This is the asymmetry that the first two rules
create, and it is the only direction in which the two halves may touch.

Nothing outside `plans/` may depend on a specific plan file — a file of work
in flight under `plans/active/` or finished work under `plans/completed/`.
The convention itself, `plans/PLANS.md`, is not a plan and is named freely;
it is the contract every plan conforms to. Plans reference artifacts;
artifacts never reference plans. The one thing plans own that outside files
need — the evidence promoting a capability card to `built` — is reached
through the card's history, not by a link from the card.

Three of the four rules above are decidable from file content, and
`tools/checks/boundary-lint` binds to the ids below rather than to this
section's prose, so that rewording a paragraph cannot silently retire a rule.
The check refuses to run — it does not pass — when this block is absent or
when its ids differ in either direction from the ones the check implements.

    no-outward-payload-reference — every path reference in a file under
      template/ resolves inside the payload, and no file there mentions this
      repository, the blueprint, or the procedure directory.
    no-unprovided-skill-target — every path reference in a file under skills/
      names something the payload ships or a procedure beside it, and no
      skill body names the payload directory.
    no-plan-file-dependency — no file outside plans/ references a plan file
      under plans/active/ or plans/completed/ that exists.

The fourth rule — a skill body may not name a harness-specific tool — has no
id, because deciding it needs a list of every tool name in every harness that
nobody can write. It stays a review job.

## Cross-cutting invariants

- Every fact has exactly one owning file, and `AGENTS.md`'s map is the
  routing registry that says which.
- The payload survives `cp -R template/. <empty-dir>/` with every
  cross-reference inside the copy resolving.
- Every artifact is markdown, operated on with shell and git only.
- Each `template/` file has a live counterpart at the same relative path from
  the repository root, with two exclusions: card files under
  `template/docs/capabilities/`, which are project content rather than
  skeletons — the payload ships a starter register and a project prunes it —
  and `.gitkeep` placeholders, whose only job is to keep an empty directory
  in git. Structure corresponds by subsequence: the template's headings,
  minus those containing fill slots, appear in the live file in the same
  order, and extra live headings are expected where filled content adds
  them. `plans/PLANS.md` is the zero-tolerance case: the two copies are
  byte-identical.
- A live card sitting at a payload card's relative path is that card's
  instantiation, and the two are copies of each other by construction: the
  live one is the generic invariant rewritten for what this project actually
  has. Shared prose between such a pair is that relationship, not
  duplication. The rewriting is why the pair is still excluded from the
  structural comparison above — an instantiation is not a fill.

## Known rough edges

- The correspondence rules above are decided by `./tools/verify`, but two
  questions around them are not decidable from text at all: whether a pair
  that corresponds structurally is about the same subject, and whether a
  live-only file should have had a counterpart. Both are tracked as `D9` in
  `docs/DEBT.md` and read by hand.
- One clause of the layer map above is not decided by anything mechanical:
  whether a skill body names a harness-specific tool. It needs a list of tool
  names that nobody can write, so it is tracked as `D10` in `docs/DEBT.md` and
  read during review of any change to a procedure.
