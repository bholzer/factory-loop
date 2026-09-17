# fast-verify

## Invariant

One command, named under Commands in `AGENTS.md`, runs every check that is
safe to run after any edit. It takes no arguments, needs no network, assumes
no setup beyond the environment command, exits zero on a clean checkout and
nonzero if any check it covers fails, and finishes within a time budget
written down next to it.

The budget is part of the invariant, not a nice-to-have. An agent that cannot
predict what a command costs stops running it, and an unrun check enforces
nothing. The number here is five seconds — twice the observed clean run,
rounded up to the plan's floor — it is stated beside the command in
`AGENTS.md`, and it is what the failing case below measures against.

This card is the enforcement point named by most other cards in this
directory, which makes one further property part of the invariant: the command
line published in `AGENTS.md` is the command the pre-commit hook runs. If the
two drift apart, every card that claims to be enforced by "the cheap
verification command" is enforced by nothing.

## Enforcement point

`./tools/verify` from the repository root, and the versioned hook
`tools/hooks/pre-commit`, which runs that same command and refuses the commit
when it exits nonzero. The hook is installed per clone with
`git config core.hooksPath tools/hooks`; a clone that has not run that line
has no gate, and `--no-verify` bypasses the one it has, which is why
`docs/DEBT.md` `D8` exists and why no rung is claimed on this gate.

There is no continuous integration to name: this repository has no remote.
The documentation-versus-gate identity is kept the cheap way — the hook
invokes the published command by name rather than restating its steps, so
there is only one definition to keep true.

## Acceptance

Passing case: from a clean checkout, run `./tools/verify` with no arguments.
It exits zero, streams each check's own report naming what that check
examined with counts greater than zero, ends with its own summary line, and
the observed wall-clock time is inside five seconds. A check whose card
requires per-unit reporting prints several such lines, which is why the
aggregator streams rather than summarizes. Record the observed time — if it
exceeds the budget, the card is not `built`, and either the budget or the
command's contents must change.

Failing case: introduce a violation of any check the command aggregates — the
cheapest is appending a line to `template/plans/PLANS.md` — run the command,
and observe a nonzero exit with the failing check's own remediation text
visible in the output, naming the offending files and the first differing
line. Revert and observe the command pass. A run that hides the underlying
message and prints only its own summary fails this case, and so does a commit
that the hook lets through while the violation stands.

## Remediation message

    fast-verify: 1 of 4 checks failed.
      template-live-drift: template/plans/PLANS.md and plans/PLANS.md differ,
      first at line 175. These two files are byte-identical by construction.
      Copy the intended version over the other and re-run.
    Fix the violations reported above and re-run ./tools/verify.

fast-verify aggregates, so its own message is a summary and a re-run
instruction; it must never swallow or truncate the remediation text of the
checks it runs. A check that exits 2 — it could not decide — is counted as a
failure here and never as a pass.
