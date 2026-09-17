# evidence-check

## Invariant

Every plan file under `plans/active/` carries the four living sections
`plans/PLANS.md` requires — Progress, Surprises & Discoveries, Decision Log,
and Outcomes & Retrospective — each present as a level-two heading with at
least one non-blank line under it, and every Progress entry whose checkbox is
ticked carries a timestamp: a parenthesised `YYYY-MM-DD` date, optionally with
the time of day, anywhere in the entry.

`plans/PLANS.md` owns what those sections mean and why a fresh-context loop
cannot work without them. This card owns only the two decidable facts — the
sections are there with text in them, and a completed entry says when. Those
are exactly the failures a session that ran out of room produces: the work
lands, the record does not, and the next session inherits a plan that no
longer describes the tree.

Decidable here means the check does not parse the plan. Headings at column
zero, non-blank lines between them, and the checkbox lines inside the Progress
section are the whole input. Fenced blocks and indented blocks are quoted
material and contribute neither headings nor entries, because a plan
reproduces the convention's own skeleton and its own command transcripts
verbatim, and a heading inside a quoted skeleton is an example rather than a
section.

An empty `plans/active/` is a passing state, not an error: the check says
there are no active plans and exits zero. A repository between plans is not a
repository with a missing record.

What this card deliberately does not decide: whether the recorded evidence is
true. "Never report a planned command as passing evidence" is a working rule
in `AGENTS.md`, enforced by a human reading the diff, and a check claiming to
verify it would be the vague wish `CARD_FORMAT.md` in this directory warns
about — passing on everything, failing on nothing, trusted anyway. The same
goes for whether a section's content is about the work it sits beside: text
under the heading is all this check can see.

## Enforcement point

`./tools/verify` from the repository root, and the versioned hook
`tools/hooks/pre-commit`, which runs that same command and refuses the commit
when it exits nonzero. There is no continuous integration to name: this
repository has no remote, and the gate's skippability is `docs/DEBT.md` `D8`.

The hook is where this check earns most of its keep. A plan's record is
cheapest to fix in the commit that changed the tree, and the failure mode the
card exists for — a session ending with the work landed and the evidence
unwritten — happens at exactly that moment.

## Acceptance

Passing case: on a clean checkout the check exits zero and prints one line per
active plan naming the four sections it found and how many Progress entries it
read, then one summary line counting the plans checked and the entries read.
With one plan in flight that is one plan line and one summary line, and the
entry count must be greater than zero — a plan whose Progress section reads as
empty to the check is a scope that passes forever.

Failing case, both halves, because they are separate code paths and a checker
that only counts headings passes the second one blind. Delete the
`## Decision Log` heading from a plan under `plans/active/` and run
`./tools/verify` — observe a nonzero exit naming the plan file and the missing
section. Restore it. Then tick the checkbox of an unfinished Progress entry in
that plan, leaving it without a timestamp, and run `./tools/verify` again —
observe a nonzero exit naming the plan file, the line number, and the entry
text. Restore it, and observe the check pass. Do both before marking the card
`built`.

Both halves mutate the plan file that the session is recording its evidence
into, so make the edit, copy the output, restore the file, and re-run
`./tools/verify` to confirm the restore before committing anything.

## Remediation message

    evidence-check: plans/active/mechanical-gate-set.md:23 — Progress entry
    "M5 — evidence-check instantiated, built and enforced; …" is marked
    complete but carries no timestamp.
      Completed entries record when the work was observed, as
        - [x] (YYYY-MM-DD HH:MMZ) M5 — evidence-check instantiated, built …
      Use the time you observed, not an estimate.

The example carries the shape of a timestamp rather than a date, because a
concrete date in a message that asks for an observation is a date someone
pastes.

For a section that is not there:

    evidence-check: plans/active/mechanical-gate-set.md is missing the
    required section `## Decision Log`.
      plans/PLANS.md requires Progress, Surprises & Discoveries, Decision Log
      and Outcomes & Retrospective in every plan. Add the heading and the
      entries the work produced; a heading with nothing under it is also a
      failure.

and for a heading with nothing under it, the same next action against the
sentence that names what is wrong:

    evidence-check: plans/active/mechanical-gate-set.md carries the required
    section `## Surprises & Discoveries` with nothing under it.

Each message names the plan, the section or the entry, and the line number
where there is one. "Plan is missing required sections" fails this bar: the
reader has to re-derive which plan and which section from a check that already
knew both.

## Per-stack hints

No toolchain applies: `ARCHITECTURE.md` states the constraint every artifact
here is written under, and the check at `tools/checks/evidence-check` is
POSIX shell with one `awk` pass per plan. The pass tracks the current heading,
records which of the four it has seen and which have text under them, and
tests the ticked checkbox lines for a parenthesised date; the shell formats
the findings. Thirty lines of awk covers both halves and stays readable for
whoever extends it.

Where a project already runs a markdown linter with custom rules, express the
same two facts there instead, so plans are checked by the same pass as the
rest of the documentation rather than by a second mechanism with its own
output format.
