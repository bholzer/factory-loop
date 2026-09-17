# blueprint-eval

## Invariant

A plain recursive copy of `template/`, plus the skills in `skills/`, is
sufficient to carry one feature from plan authoring to an executed milestone
with recorded evidence, in every harness this project claims to support, with
no access to this repository.

The last clause is the whole point. Every other check here reads the payload
from inside the repository that produced it, where a dangling reference still
resolves and an unstated assumption is still in someone's head. This one reads
it from where a target project sits: an empty directory with a copy and
nothing else.

Supported harnesses are the ones `GOALS.md` names as a success condition.
Adding a harness to that list without running this trial claims a result
nobody observed.

## Enforcement point

A trial run, gating any change under `template/` or `skills/`. Mechanization
is bounded by the harnesses themselves — driving one non-interactively may not
be possible — so the trial is a partly manual procedure with its checkable
parts scripted: the copy, the fill check, reference resolution inside the
copy, and skill-file layout. Until it is built, the gate is a human reading
the changed payload and judging, which is the gate this card retires. Its
status is in `docs/capabilities/index.md`; the rung it gates is in
`docs/MATURITY.md`.

## Acceptance

Passing case, run once per supported harness: copy the payload into an empty
git repository, install the skills, and in that repository — using only that
harness — invoke the bootstrap skill, fill the skeletons for a trivial toy
project, author a plan, then execute its first milestone in a fresh session.
Observe all five of:

- the harness discovered every skill with no per-harness edit to any file;
- after bootstrap, the fill check over the copy's skeleton-derived artifacts
  comes back empty;
- every path reference inside the copy resolves inside the copy;
- the executed milestone's living sections carry output that was observed in
  that session, not restated from the plan;
- the session stopped after one milestone rather than continuing.

Record harness, version, date, and outcome in the plan that runs the trial.
The card stays a specification and holds no results.

Failing case: rename `skills/harness-init/SKILL.md` to
`skills/harness-init/README.md`, re-run the trial in one harness, and observe
it fail naming the harness and the skill that could not be discovered.
Restore the name. A second failing case worth demonstrating, because it is the
class of defect the trial exists to catch: delete one file from `template/`
that `template/AGENTS.md`'s map names, run the reference-resolution part
against a fresh copy, and observe it name the dangling map entry.

## Remediation message

    blueprint-eval: Codex discovered 5 of 6 skills — `harness-init` is
    missing. Expected skills/harness-init/SKILL.md with `name` and
    `description` frontmatter; found skills/harness-init/README.md.
    Skill layout is fixed by every supported harness at once; restore the
    file name rather than adapting one harness.

    blueprint-eval: the copied payload references `docs/MATURITY.md`, which
    the copy does not contain (cited by AGENTS.md:50).
    Fix this in template/, never in the copy — the copy is a scratch
    artifact and the next trial rebuilds it.

Both messages name the harness or the copy, the exact missing artifact, and
where the fix belongs. The second one exists because the instinct when a trial
fails is to patch the trial repository, which produces a green run and leaves
the payload broken.

## Per-stack hints

The scriptable parts need only shell and git: `cp -R template/. "$T"/`,
`grep -rn` for the fill markers, a reference walk over backticked paths, and
`test -e` per skill file. The harness-driven parts are a written checklist with
one line per observation above, filled in per harness. Prefer running the trial
in a temporary directory outside this repository, so that a stray reference to
an ancestor path fails loudly instead of resolving by accident.
