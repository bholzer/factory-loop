# The check protocol

## What this file owns

Every mechanical check this repository runs on itself is one executable
under `tools/checks/`, and this file is the contract any of those
executables — present or future — conforms to: how a check is invoked,
what its exit codes mean, the shape of what it prints, the format of the
allowlist it may read, how the aggregator that runs them all behaves, and
how the time budget published beside the command is set. What each check
decides, and how the three runnable commands differ, is
`docs/specs/mechanical-checks.md`'s subject; the inventory of what lives
under `tools/` is owned by the check layer entry in `ARCHITECTURE.md`;
and the remediation text a given check prints is owned by that check's
card under `docs/capabilities/`. None of that is restated here.

## Invocation

A check is written in POSIX `sh` and may call `awk`, `grep`, `sed`,
`sort`, `find`, `cmp`, and `test` — no other dependency, no bashisms, no
network. It takes no arguments: everything a run needs is in the tree, so
two people running the same commit read the same verdict. Its first act
is to change to the repository root:

    cd "$(git rev-parse --show-toplevel)" || exit 2

which is why a check behaves identically run from a subdirectory, run by
the aggregator, and run by the versioned hook `tools/hooks/pre-commit`.

## Exit meanings

Exit 0 means the invariant holds. The check's last line is its own name,
a colon, and an `ok` report counting what was examined:

    <check-name>: ok — <what was examined, with counts>

The counts are load-bearing: they are how a reader tells a pass over a
real scope from a pass over an empty one. Whether an empty scope is a
legitimate pass or an undecided run is each card's call to make — the
plans-in-flight check passes over an empty directory and says so on its
card — and the counts keep either answer honest.

A check whose card requires per-unit reporting prints one name-prefixed
line per unit before the summary. Two cards require it today:
`docs/capabilities/template-live-drift.md`, one line per pair under
`template/` stating the comparison applied, and
`docs/capabilities/evidence-check.md`, one line per active plan stating
the sections found and the entries read. The self-test
`tools/checks/loop-runner` reports the same way — one line per scenario
it drives, five today — as that script's own report shape rather than a
card clause, so a full run streams unit lines from three checks.

Exit 1 means a violation. The check prints one block per violation, built
by substituting the real offender — the file, the line, the target, the
rule — into the remediation text its card publishes, and closes with a
line counting the violations found. A per-unit reporter prints a unit's line
only when that unit is clean, so every unit is described exactly once
either way: a remediation block or an ok line, never both.

Exit 2 means the check could not decide — an input it needs is missing,
an allowlist entry carries no reason, a discovered scope came back empty
where the card does not permit that. It prints a line naming the reason
and a second line naming the next action, because a refusal that does not
say how to restore the check turns one blocked commit into two:

    <check-name>: cannot run — <reason>
    <what to restore or correct, then re-run>

The aggregator counts an undecided check as a failure, never as a pass;
that rule sits on `docs/capabilities/fast-verify.md`, with the reasoning.

## The aggregator

`tools/verify` runs the checks in a fixed order written into the script
as a literal list — never a glob, so output order cannot depend on the
filesystem and a check nobody wrote into the list cannot run silently.
Seven checks run today, in the order the script's list spells them:
`scaffolding-markers`, `template-live-drift`, `doc-integrity`,
`prose-duplication`, `evidence-check`, `boundary-lint`, `loop-runner`.

The aggregator begins at the repository root the same way a check does,
streams every child's output unmodified — the no-swallowing rule is part
of `docs/capabilities/fast-verify.md`'s own acceptance — and keeps going
past a failure, so one run reports every violation rather than the first.
A listed check that is missing or not executable is reported by the
aggregator itself, as a `cannot run` line plus a restore instruction, and
counted as a failure. A clean run ends

    fast-verify: <M> of <M> checks passed (<seconds>s).

and exits 0; a run with failures ends

    fast-verify: <N> of <M> checks failed.
    Fix the violations reported above and re-run ./tools/verify.

and exits 1.

## Allowlists

A check that needs a recorded exception reads a file under `tools/allow/`
named after itself. One entry per line: a key, two spaces, a `#`, and a
one-line reason. Blank lines and lines beginning with `#` are ignored. An
entry with no reason is refused — the check exits 2 rather than honor it,
because an exception nobody justified is an exception nobody can review.
An entry whose key no longer matches anything is stale: it is counted in
the passing summary line so it decays visibly, and it never fails the
check, whose card states one invariant and staleness is not it. The shape
of the key is owned by the card whose check reads the file —
`docs/capabilities/doc-integrity.md` and
`docs/capabilities/prose-duplication.md` each publish theirs inside their
remediation text.

## The budget

`AGENTS.md` publishes a number of seconds beside the command, because a
command whose cost cannot be predicted stops being run, and an unrun
check enforces nothing — the budget is part of
`docs/capabilities/fast-verify.md`'s invariant, and the current number
lives beside the command and on that card, nowhere else. The rule that
sets it: twice the observed clean run, rounded up, and never above sixty
seconds. Any commit that changes the check set re-measures a clean run,
and if the published number no longer holds, that same commit corrects
the budget line rather than leaving the drift for a reader to find.
