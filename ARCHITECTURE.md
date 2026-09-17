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

- Lives in: `skills/<name>/SKILL.md`. Two exist —
  `skills/plan-author/SKILL.md` and `skills/plan-execute/SKILL.md`; the
  bootstrap, capability-building, doc-gardening, and retrospection
  procedures the next entry names do not exist yet.
- Owns: the reusable procedures an agent invokes — bootstrap, plan authoring,
  plan execution, capability building, doc gardening, retrospection.
- Does not: carry rules that belong to an artifact. A skill references
  `plans/PLANS.md` and the docs tree; it never restates their content.

## Layer map and dependency rules

Two of the four components are portable payload — `template/` and `skills/` —
and portability is exactly a dependency restriction, so the layer map is
stated as rules a reader can decide by reading one line of text.

Nothing under `template/` may name a path outside `template/`, and nothing
there may mention this repository, the blueprint, or `skills/`. A target
project receives that directory's contents with no ancestor context, so an
outward reference is a dangling reference by construction.

Nothing under `skills/` may name a path that the payload does not provide.
The allowed targets are the artifacts a bootstrapped project has —
`AGENTS.md`, `GOALS.md`, `ARCHITECTURE.md`, `docs/PRINCIPLES.md`,
`docs/MATURITY.md`, `docs/DEBT.md`, the three `docs/` directories and their
format documents, `plans/PLANS.md`, `plans/active/`, `plans/completed/` —
plus sibling skills by `skills/<name>/SKILL.md`. Directories are namable;
the project-specific files inside them are not, because a decision record,
a spec, a card, or a plan file exists only in the project that wrote it.
Skill bodies may not name `template/`, because a target project does not
receive it, and may not name a harness-specific tool, because the same file
must work in every harness.

The live root artifacts may name `template/` freely: the payload is this
project's subject matter. This is the asymmetry that the first two rules
create, and it is the only direction in which the two halves may touch.

Nothing outside `plans/` may depend on a specific plan file. Plans reference
artifacts; artifacts never reference plans. The one thing plans own that
outside files need — the evidence promoting a capability card to `built` — is
reached through the card's history, not by a link from the card.

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

## Known rough edges

- Template ↔ live correspondence is maintained by hand; no check enforces the
  counterpart existence, the structural subsequence, or the byte identity
  above. The mechanism is specified in
  `docs/capabilities/template-live-drift.md` and its absence is tracked as
  `D1` in `docs/DEBT.md`.
