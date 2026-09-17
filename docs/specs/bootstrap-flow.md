# The bootstrap flow

## What this repository offers an outside reader

Two things, both installable by copying files. The payload in `template/` is
an artifact set a project receives and fills in: an agent guide, goals,
architecture, a principles file, a maturity ladder, a debt register, a
decision-record directory with its format, a behavior-spec directory with its
reflection rule, a capability register with six starter cards and their
format, and the plan convention. The procedures in `skills/` are six
markdown files an agent reads and follows: `harness-init`, `plan-author`,
`plan-execute`, `doc-garden`, `retro`, `capability-build`.

Nothing is executed to install either one. There is no generator, no
templating engine, and no dependency: the observable operation is a recursive
copy followed by an editing pass that replaces the payload's fill slots with
project content and deletes its authoring guidance.

## What a project has after the flow runs

An agent opening the project cold reads one file, `AGENTS.md`, and from its
map reaches every other artifact and the two or three commands the project
actually runs. Multi-file work goes through a plan under `plans/active/` that
a later session can execute from that file and the worktree alone. Every
invariant the project wants enforced exists as a card in
`docs/capabilities/`, with its status in the register beside it, and nothing
is enforced mechanically until someone builds one — the ladder in
`docs/MATURITY.md` starts at L0 and moves only by the rule stated there.

Each procedure is invoked by reading the file at its mapped path. That works
in any agent environment; automatic surfacing does not, and `docs/DEBT.md`
`D4` carries why.

## What holds today, and what has not been observed

Established by running the checks: the payload survives a plain recursive
copy into an empty directory with every reference inside the copy resolving,
and all six procedures reference only paths the copy provides. This
repository is itself the filled form of that payload, which is what makes
divergence between the two halves visible at all.

Not established: nobody has bootstrapped a different project from it, and no
feature has been driven through the plan loop anywhere but here. The trial
that would settle it is specified in `docs/capabilities/blueprint-eval.md`
and tracked as `D7` in `docs/DEBT.md`. Until it runs, every claim on this
page is about a tree of files that has been read, copied, and checked — not
about a project that has lived with it.
