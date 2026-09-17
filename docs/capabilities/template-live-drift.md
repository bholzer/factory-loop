# template-live-drift

## Invariant

The correspondence invariant in `ARCHITECTURE.md` holds across the whole tree.
Stated as the three comparisons a check must make:

- **Counterpart existence.** Every file under `template/` has a live
  counterpart at the same relative path from the repository root.
- **Byte identity.** `template/plans/PLANS.md` and `plans/PLANS.md` are
  byte-identical.
- **Structural correspondence.** For every other pair, the template file's
  headings, minus those containing a `{{FILL: …}}` slot, appear in the live
  file in the same order. Extra live headings are expected: filled content
  adds them.

Two exclusions, both of which a naive walk gets wrong. Card files under
`template/docs/capabilities/` are excluded from counterpart existence,
because a card is project content — the payload ships a starter register that
a target project prunes, and this project's register is its own.
`docs/capabilities/index.md` is *not* excluded: it is a skeleton and has a
live counterpart. Directory placeholders named `.gitkeep` are also excluded,
since their whole purpose is to keep an empty directory in git, and a live
directory holding real files has no need of one.

The check runs template to live only. Live-only files — `docs/decisions/`
records, files in `plans/active/`, and cards this project wrote for itself —
are not drift, and the reverse comparison does not exist.

## Enforcement point

The cheap verification command under Commands in `AGENTS.md`, where it
replaces the single `cmp` line that covers only the byte-identity case today,
and the same command in continuous integration. Failure refuses the change.
The commit that makes the check real also deletes the hand-walk instructions
from `docs/DEBT.md`, since the debt is the absence of this check.

## Acceptance

Passing case: on a clean checkout the check exits zero and prints one line per
pair stating which comparison it applied — byte identity, subsequence, or
excluded — plus a total. The totals must account for every file under
`template/`, so an excluded file is reported as excluded rather than skipped
silently; silent skipping is how an exclusion list grows until the check
covers nothing.

Failing cases, all three of which must be demonstrated before the card is
`built`, because they are three separate comparisons:

- Delete the `## Register` heading from `docs/capabilities/index.md`, run the
  check, observe a nonzero exit naming that pair and the missing heading.
  Restore.
- Append a line to `template/plans/PLANS.md` only, run the check, observe a
  nonzero exit naming both paths and the first differing line. Restore.
- Create `template/docs/EXAMPLE.md`, run the check, observe a nonzero exit
  naming the missing live counterpart `docs/EXAMPLE.md`. Delete the file.

## Remediation message

    template-live-drift: docs/capabilities/index.md is missing the heading
    `## Register`, which template/docs/capabilities/index.md declares.
    Add the heading to the live file, or remove it from the template if the
    section is genuinely gone — a generic change belongs in both halves.

    template-live-drift: template/plans/PLANS.md and plans/PLANS.md differ,
    first at line 42. These two files are byte-identical by construction.
    Copy the intended version over the other and re-run.

    template-live-drift: template/docs/EXAMPLE.md has no live counterpart at
    docs/EXAMPLE.md. Either instantiate it for this project, or — if it is a
    card file or a directory placeholder — check it against the exclusions in
    docs/capabilities/template-live-drift.md.

Each message names the pair, the specific divergence, and which half to
change. A message that reports only "template and live differ" leaves the
reader to redo the comparison by hand, which is the state this card exists to
end.

## Per-stack hints

No toolchain applies: this repository is markdown operated on with shell and
git. `find template -type f`, `cmp` for the one byte-identical pair,
`grep '^#'` piped through a subsequence walk for the rest, and `test -e` for
counterpart existence. That is under fifty lines of `sh` with no dependencies,
which matters because the check has to keep working in a repository that
deliberately has no build.
