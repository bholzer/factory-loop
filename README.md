# harness-blueprint

A starter kit for projects where coding agents do most of the work. It has two
parts. The first is a set of markdown files a project copies in and fills out:
goals, architecture, principles, a plan format, a debt register, and specs for
the checks the project wants. The second is six written procedures an agent
reads and follows to set that project up, plan work, carry out the work, build
checks, and keep the docs honest over time.

There is no program to install. Everything is markdown, and the only tools it
assumes are a shell and git. It does not care what language the project uses or
which agent product drives the sessions. It has been run end to end in Claude
Code, Codex CLI, and omp.

This repository also uses the kit on itself. The files at its root are the kit
filled out for this project, so this repository is the kit's first user as
well as its source.

## Contents

- [The problem it solves](#the-problem-it-solves)
- [What's in the repository](#whats-in-the-repository)
- [Core concepts](#core-concepts)
- [Quickstart: adopting it in your project](#quickstart-adopting-it-in-your-project)
- [Running a project day to day](#running-a-project-day-to-day)
- [Working on this repository](#working-on-this-repository)
- [What is proven and what isn't](#what-is-proven-and-what-isnt)
- [Glossary](#glossary)
- [Where to read next](#where-to-read-next)

## The problem it solves

An agent session starts with nothing. Whatever you told the agent yesterday,
whatever it figured out an hour ago in another session, is gone. If a decision
lives only in a chat log, in a code review thread, or in someone's head, the
next session can't see it and will make the decision again, often differently.

So this kit treats the repository as the only memory the project has. Goals,
constraints, plans in progress, the evidence that work happened, and the
reasons behind choices all live in files. A new session reads those files and
picks up where the last one stopped. The rule in [GOALS.md](GOALS.md) is blunt
about it: anything decided outside the repo has to land in the repo to exist.

The kit covers five concerns, which [GOALS.md](GOALS.md) calls subsystems:

1. **Knowledge.** Where each fact lives, and a small map that routes a reader
   to it. This is `AGENTS.md` plus the `docs/` tree.
2. **Constraints.** Rules that matter are written as specs for mechanical
   checks rather than left to reviewers' memory. This is
   `docs/capabilities/`.
3. **Feedback.** An agent can check its own work with one cheap command
   instead of asking a person whether it's right.
4. **Process.** Multi-step work runs under a plan file that any fresh session
   can pick up, one milestone at a time. This is `plans/`.
5. **Entropy.** Drift and debt get tracked and cleaned up from the first
   commit, not after the docs have already rotted. This is `docs/DEBT.md` plus
   the gardening and retrospective procedures.

## What's in the repository

```
harness-blueprint/
├── template/            The payload: what a target project copies in.
│   ├── AGENTS.md          Skeletons with fill-in slots and authoring notes.
│   ├── GOALS.md
│   ├── ARCHITECTURE.md
│   ├── docs/              Principles, maturity ladder, debt register,
│   │                      decision and spec directories, six starter
│   │                      capability cards and the card format.
│   └── plans/PLANS.md     The plan convention, shipped word for word.
├── skills/              The six procedures, one SKILL.md each.
│   ├── harness-init/      Set up a repository that has none of this yet.
│   ├── plan-author/       Write a plan for multi-file or multi-session work.
│   ├── plan-execute/      Carry out one milestone of a plan, then stop.
│   ├── capability-build/  Turn a check spec into a working check.
│   ├── doc-garden/        Sweep the docs for drift and fix it.
│   └── retro/             Learn from finished work and file the lessons.
│
│   Everything below is this project's own filled-out copy of the payload.
├── AGENTS.md            The map every agent session reads first.
├── CLAUDE.md            One line, "@AGENTS.md", for harnesses that look for it.
├── GOALS.md             Purpose, outcomes, scope, non-goals, constraints.
├── ARCHITECTURE.md      Components and which way references may point.
├── docs/
│   ├── PRINCIPLES.md      Four rules that outrank convenience.
│   ├── MATURITY.md        The autonomy ladder and the current rung.
│   ├── DEBT.md            Known debt, each row with what ends it.
│   ├── decisions/         One short file per lasting decision.
│   ├── specs/             How the system behaves today.
│   ├── capabilities/      Check specs ("cards") and their status register.
│   └── guide/             Longer explanations for people.
├── plans/
│   ├── PLANS.md           The plan convention (identical to the template's).
│   ├── active/            Plans in progress. Empty right now.
│   └── completed/         Finished plans, kept as a record.
└── tools/               Shell scripts. These stay here; projects don't get them.
    ├── verify             Runs every check. The cheap command.
    ├── checks/            One script per check.
    ├── allow/             Allowlists for the checks that need judgement calls.
    ├── hooks/pre-commit   Refuses commits that fail verify.
    ├── blueprint-eval     Builds a throwaway trial project from the payload.
    └── loop-runner        Drives a plan in another repo, one session per milestone.
```

Two things about this layout trip people up.

First, `template/` and the root look alike on purpose. `template/AGENTS.md` is
a blank form. The root `AGENTS.md` is that form filled out for this project.
When you improve something generic, you usually have to change both.

Second, `tools/` is not part of what a project receives. Those scripts assume
this repository's exact file set. A project gets check *specs* and builds its
own checks in its own stack.

## Core concepts

### The repository is the only memory

Every rule in the kit comes back to one assumption. The agent's context window
is a scratchpad that gets thrown away when the session ends. Nothing in it
carries over. That's why plans are written so a stranger can resume them, why
evidence goes into the plan file instead of a chat reply, and why the kit keeps
insisting on files over conversation.

It also explains why a session does one milestone and stops. A long session
that runs until its context fills up, or until the harness compresses the
history, loses track of what it was doing. A short session that writes down
what it did before it exits leaves a clean handoff.

### AGENTS.md is a map

`AGENTS.md` is the first file every agent session reads, so every line in it
gets paid for again in every session. It stays under about 100 lines. It has a
one-paragraph description of the project, a map of where each kind of fact
lives, the exact commands agents run, and the working rules.

It is deliberately not a manual. When something wants more than a line, it
moves into `docs/` and leaves a one-line pointer behind. Harnesses that don't
read `AGENTS.md` natively get a tiny shim file (here, `CLAUDE.md` containing
`@AGENTS.md`) that points to it and holds nothing else.

This README is for people. Agents start at `AGENTS.md`.

### One owner per fact

Every fact lives in exactly one file. Other files that need it link to that
file instead of repeating it. The failure this prevents is the common one: a
fact copied into three places, updated in one, and quietly wrong in the other
two. [docs/PRINCIPLES.md](docs/PRINCIPLES.md) states this rule, and a check
(`prose-duplication`) flags any run of eight words that appears in two
different files.

In practice, when you want to write something down, the question is "which
file owns this?" The map in `AGENTS.md` answers it:

| You want to record... | It goes in |
| --- | --- |
| What the project is for, what's out of scope | `GOALS.md` |
| The components and which may depend on which | `ARCHITECTURE.md` |
| A rule that should override local convenience | `docs/PRINCIPLES.md` |
| Why a lasting choice was made over the obvious alternative | a new file in `docs/decisions/` |
| How the system behaves right now | a file in `docs/specs/` |
| An invariant a machine should enforce | a card in `docs/capabilities/` |
| Work you noticed but aren't doing now | a row in `docs/DEBT.md` |
| Work that spans files or sessions | a plan in `plans/active/` |
| How much the agent may do unsupervised | `docs/MATURITY.md` |

Two exceptions are allowed. A check spec has to state the rule it checks, and a
format document has to show the shape it defines. Both name the owning file
right next to the restatement. The guide pages in `docs/guide/` and this README
also retell facts, but as tours: they point at the owner, and the owner wins
if they disagree.

### The payload and its two kinds of file

Everything in `template/` falls into one of two groups.

**Skeletons** are forms to fill out. They contain two kinds of markers,
defined in `template/AGENTS.md`:

- `{{FILL: ...}}` is a slot to replace with real content.
- An HTML comment starting with `GUIDANCE` is an authoring note. Each one says
  what the file is responsible for, what belongs somewhere else, and which
  failure the file guards against. Read it, fill the file, delete the note.

A skeleton is finished when `grep -rn '{{FILL' <file>` and
`grep -rn GUIDANCE <file>` both come back empty.

**Format documents** are copied over unchanged and never edited:
`docs/capabilities/CARD_FORMAT.md`, `docs/decisions/DECISION_FORMAT.md`, and
`plans/PLANS.md`. A project doesn't edit the format it's supposed to follow.

The starter capability cards are a third case. They are project content, so a
project deletes the ones that can't apply. A project with no layers to violate
has no use for a layering check, for example. This repository dropped the
`isolated-env` card because a repository of markdown has no runtime to isolate.

### Direction of reference

The two halves of this repository may only point at each other one way, and
[`ARCHITECTURE.md`](ARCHITECTURE.md) writes that down as a layer map:

- Files in `template/` only point at other files in `template/`. They never
  mention this repository or `skills/`, because a project gets them with none
  of that around them, and an outward pointer would lead nowhere.
- Nothing in `skills/` may reference a path the payload doesn't provide, and a
  skill may not name `template/`, since projects never receive that directory.
- The live root may talk about `template/` freely. The payload is what this
  project is about.
- Only files inside `plans/` may point at an individual plan file. Plans cite
  the docs, and the docs never cite plans, since a finished plan records how
  something changed rather than how things are now.

A fifth rule says a skill may not name a tool specific to one harness. No
script can check that, since it would need a list of every tool in every
harness, so reviewers check it by reading.

The three rules a script can decide carry IDs in an indented block in
`ARCHITECTURE.md`, and the `boundary-lint` check binds to those IDs rather than
to the prose. If someone rewords the section and drops a rule, the check
refuses to run instead of silently passing.

### ExecPlans

Any work that touches more than one file or needs more than one session runs
under an ExecPlan: a single markdown file in `plans/active/`, written so that
someone with only that file and the working tree could finish the job.
[`plans/PLANS.md`](plans/PLANS.md) is the full convention. The points that
matter most:

- **Self-contained.** The plan explains every term it uses, names files by full
  path, and repeats any assumption it relies on. "As discussed" and "see the
  other doc" don't count.
- **Milestones.** The work is cut into milestones, each small enough to finish
  and verify in one session, roughly one pull request's worth. Each milestone
  says what will exist afterward and how to observe it.
- **Four living sections.** Every plan keeps `Progress` (a checklist with
  timestamps), `Surprises & Discoveries`, `Decision Log`, and
  `Outcomes & Retrospective` up to date as work happens. The `evidence-check`
  check fails if an active plan is missing one, or ticks an item without a
  timestamp.
- **No nesting.** Oversized work is split into two separate plans, and the
  first one names its successor. There are no sub-plans or per-milestone
  files.

When a plan finishes, four things happen in order. The session writes the
retrospective. Any change in observable behavior goes into `docs/specs/`
(the "reflection rule" in `docs/specs/index.md`). Decisions that will keep
binding future work graduate from the plan's Decision Log into
`docs/decisions/`. Then the file moves to `plans/completed/`.

### One milestone per session

This rule lives in the house rules of `plans/PLANS.md`. The session starts by
reading the map, the plan, and whatever files that plan lists. It takes the
first milestone not yet done, updates the living sections with what it
observed, commits, and stops. It does not roll on to the next milestone, even
if it has room. Whatever drives the loop (a person, or `tools/loop-runner`)
starts the next session.

If a milestone turns out too big mid-session, the session splits it in the
`Progress` list ("completed: X; remaining: Y"), logs the split, and exits
before the context runs out.

### Evidence means observed

A plan only records what someone actually saw. "Run `./tools/verify` and it
should pass" is a plan. "Ran `./tools/verify`, exit 0, `7 of 7 checks passed`"
is evidence. You never copy an expected result into the record as though it
happened. Failed attempts stay in the record, and corrections are appended
rather than rewritten.

No script can tell whether quoted output was really produced, so this one is
enforced by a person reading the diff.

### The six procedures

The files under `skills/` are step-by-step procedures an agent reads and
follows. They only ask for file reads, shell commands, and git, which is why
one copy of each works under every harness. Each is for one occasion and each
has a clear stopping point.

| Procedure | Use it when | It stops when |
| --- | --- | --- |
| `harness-init` | A repository has no `AGENTS.md` or `plans/PLANS.md` yet | The artifacts are filled, the procedures installed, everything verified and committed. It never starts the project's first feature or plan. |
| `plan-author` | Work will span more than one file or session | The plan is committed. It implements nothing. |
| `plan-execute` | A plan has an unfinished milestone | That one milestone is done, recorded, and committed. |
| `capability-build` | A card describes a check nobody has built | The check exists, has been seen failing on a real violation, is wired into the cheap command or hook, and the register is updated. |
| `doc-garden` | The docs may have drifted: a convention changed, a plan finished, enough changes landed, or you're about to lean on the docs | Each finding has its smallest fix, in a session of its own. |
| `retro` | A plan finished, or one episode was costly enough to learn from | Every lesson worth keeping has landed in the file responsible for it, or has a stated reason for changing nothing. |

The procedures cite each other, so they're installed as a set. A project never
drops one because it looks unneeded.

Sessions get to a procedure by opening its file. The live trials turned up no
skill-loading feature common to all three harnesses, and a skill that loads by
matching its description can quietly not load. Naming the path in the prompt
works the same way everywhere. Decision record 0023 in `docs/decisions/` has
the reasoning.

### Capability cards

A rule worth enforcing gets a one-page spec called a capability card, in
`docs/capabilities/<name>.md`.
[`docs/capabilities/CARD_FORMAT.md`](docs/capabilities/CARD_FORMAT.md) defines
its five sections:

1. **Invariant.** What must always hold, stated so a violation is a yes/no
   question. "Keep the core clean" is a wish. "No file under src/core imports
   anything from src/adapters" is an invariant.
2. **Enforcement point.** Where the check runs (the cheap command, a pre-commit
   hook, CI) and what it blocks.
3. **Acceptance.** A passing case and a failing case. The failing case is a
   concrete bad edit someone can make on purpose, plus the failure output it
   has to trigger.
4. **Remediation message.** The exact text the check prints on failure. It
   names the rule, the offending file and line, and what to do next. This is
   mandatory, because an agent reads a failure message at the exact moment it's
   doing the wrong thing. That makes it the one piece of documentation
   guaranteed to be read.
5. **Per-stack hints.** Optional suggestions for mechanisms.

Cards describe what must hold, never how to build the check. A project builds
the check with tools it already runs, whether that's its linter, its test
suite, CI, or a small script.

A card's status is written in exactly one file,
[`docs/capabilities/index.md`](docs/capabilities/index.md), and nowhere else:

- `specced` means the card is complete and nothing runs yet.
- `built` means the check runs on demand. Getting here requires the
  **promotion bar**: someone makes the violating change from the card's failing
  case, runs the check, watches it fail with the remediation text showing, then
  reverts and watches it pass. A check nobody has seen fail isn't known to check
  anything.
- `enforced` means it runs somewhere it can't be skipped casually: the cheap
  command, a hook, or CI.

The evidence from that demonstration goes into the plan that built the check.
The card stays a spec.

### The maturity ladder

[`docs/MATURITY.md`](docs/MATURITY.md) defines how much the agent may do
without a person, in four rungs:

| Rung | Name | What changes |
| --- | --- | --- |
| L0 | Human-gated | The agent does the work. A person reads every change before it lands. |
| L1 | Self-verifying | The agent may land changes fully covered by enforced checks. Anything else still needs a person. |
| L2 | Agent-reviewed | A reviewing agent reads changes against the written artifacts first. People handle escalations and a sample. |
| L3 | Bounded autonomy | For named, mechanically bounded classes of work, the agent plans, builds, verifies, and merges alone. |

Moving up takes three things. Every card that gates the rung must be
`enforced`. Each must have passed on twenty landed changes in a row, with no
check skipped or switched off along the way. And a named person signs off on
the move in the commit that changes the rung. A good track record from the
agent counts for nothing here; only checks that catch the mistake every time
do.

The rung drops one step at once if a gated mistake gets through, a gating check
gets disabled or made advisory, or a spot check shows a gating check failing to
fail. Climbing back requires fixing the gap, showing the failing case, and
restarting the count.

This repository is at L0. Its L1 cards are all enforced, but it doesn't yet
have the twenty-change green run, and its only gate is a local hook anyone can
skip (`D8` in `docs/DEBT.md`). `docs/MATURITY.md` explains the rest.

### Debt, decisions, and specs

Three directories hold the project's long-term memory, and each has a narrow
job.

**`docs/DEBT.md`** is the register of things known to be wrong or missing. Each
row has an ID, the item, where it lives, why it's waiting, and what event
should end the wait. A session that notices something outside its milestone
writes a debt row instead of fixing it on the side. When the debt is paid, the
row is deleted, not struck through.

**`docs/decisions/`** holds one short file per durable decision, named
`NNNN-short-slug.md`. Each explains why the choice was made over the
alternative that looks obvious. These exist to stop the "confident revert": an
agent sees something that looks wrong, can't find a reason, fixes it, and
brings back the problem the odd shape was avoiding. Decision records are the
one exception to "delete what has no reader". Nobody edits them after
the fact, and a record that's been replaced still reads as it was written.

**`docs/specs/`** describes how the system behaves right now. Specs get
overwritten freely, since they describe the present and git keeps the past.
Every finished plan either updates a spec or states that its outcome was
purely internal.

### Keeping entropy down

Docs rot unless something pushes back. The kit pushes back in three ways:

- **Checks** catch the drift a machine can see: broken paths, copied prose,
  leftover fill slots, plans missing their record.
- **`doc-garden`** is a recurring sweep for what checks can't see: statements
  that stopped being true, stale debt rows, active plans left behind after
  `plans/PLANS.md` changed. It gets a session to itself, separate from any
  milestone's commits, since a cleanup pass tends to grow.
- **`retro`** runs when a plan finishes. It compares what the plan meant to do
  with what its evidence shows, and routes each lesson to one owning file.

And one principle: content with no reader left gets deleted. Authoring notes go
once a skeleton is filled, debt rows go once paid, wrong prose goes once it's
wrong.

## Quickstart: adopting it in your project

The full walkthrough is [docs/guide/adopting.md](docs/guide/adopting.md). The
short version follows.

### 1. Gather what you need

- A checkout of this repository. You copy from it once and the target never
  references it again.
- A target repository. An empty one is the tested case. For an existing
  codebase, read `D6` in `docs/DEBT.md` first, because the layering check has
  no retrofit story yet.
- One agent harness: Claude Code, Codex CLI, or omp have all been run.
- Answers to the interview questions below, or the project owners on hand.

### 2. Seed the target

Installing is a copy. The contents of `template/` go to the target's root, and
`skills/` goes beside them, unchanged.

```sh
BLUEPRINT=/path/to/harness-blueprint
cd /path/to/my-project            # an empty git repo, or run `git init` first
cp -R "$BLUEPRINT/template/." .
cp -R "$BLUEPRINT/skills" ./skills
git add -A
git commit -m "Receive the payload"
```

Commit the copy exactly as it arrived, before any session edits it. Every later
edit can then be read as a diff from the untouched copy.

To rehearse this without touching a real project, `./tools/blueprint-eval new
<label>` does the same copy into a fresh repository under
~/blueprint-trials (or `$BLUEPRINT_TRIAL_ROOT`) and prints where it went.

### 3. Run the bootstrap session

Start one agent session in the target. Tell it to find the bootstrap procedure
and follow it, and give it the owners' answers in the same prompt. You don't
need to point it at the file. In every recorded trial, the session found
`skills/harness-init/SKILL.md` on its own within its first two commands.

The interview (step 2 of the procedure) asks for:

- what the project is for
- who uses it, or what system depends on it
- what success looks like
- what's deliberately excluded
- ideas someone will suggest that you already know you don't want
- rules that beat convenience when they conflict
- open questions nobody can answer yet
- optionally, where this kit came from, so the project's `GOALS.md` can record
  a pointer back to this guide

A prompt might look like:

```
Find the bootstrap procedure in this repository and follow it.
Owner answers:
- Purpose: a command-line tool that records labels and prints counts.
- Serves: one person on one machine.
- Success: `tally add` and `tally report` work, with a test command.
- Out of scope: networking, multiple users.
- Non-goals: a GUI; a plugin system.
- Constraints: standard library only.
- Unknowns: where the data file should live by default.
```

Any question left unanswered gets recorded as an unknown. The procedure treats
an invented answer as worse than a missing one, since everything later would be
graded against a made-up goal.

The session then proposes which artifacts and cards to keep, copies and fills
every skeleton (map last), sets the ladder to L0, seeds `docs/DEBT.md` with what
it found, installs the procedures, makes them reachable for your harness,
verifies, commits, and stops.

### 4. Read the report

The session ends with a report covering:

- what it created
- which content came from your answers rather than from the tree
- what it left out, with a reason for each omission
- what it put in the debt register
- whether it added an entry-point file (like `CLAUDE.md`) for a harness that
  doesn't read `AGENTS.md`, or why none was needed
- anything it set up so your harness finds the procedures by itself, or the
  reason it set up nothing

Reachability varies by harness. The procedures stay in `skills/` no matter
what. In the recorded runs, the Claude Code session added a .claude/skills
link aimed at `skills/` and committed it. Codex CLI installed nothing, since it
only reads directories on the user's machine and a bootstrap never configures
the machine. omp went different ways on different runs. Reading the file by path always works.

In an empty project, expect the reference check in step 10 to report
unresolved paths. `ARCHITECTURE.md` now describes code that doesn't exist yet.
Those paths name code that's on its way, and the procedure logs them as a debt
row. A report claiming a clean walk on a greenfield project would be the
actual failure.

For sizing: across the recorded trials, bootstrap plus a first plan plus that
plan's first milestone took 20 to 29 minutes of unattended session time per
harness. The bootstrap alone ran about 6 to 11 minutes.

### 5. Start real work

The bootstrap deliberately stops before any real work. Your next steps are new
sessions:

1. Author the first plan with `skills/plan-author/SKILL.md`.
2. Execute its milestones one session at a time with
   `skills/plan-execute/SKILL.md`.
3. When a card is worth building, build it with
   `skills/capability-build/SKILL.md`.

## Running a project day to day

[docs/guide/operating.md](docs/guide/operating.md) is the full runbook. The
core of it:

### The prompts

Three prompts drive nearly everything. Each one names a file to follow, so the
prompt carries no instructions of its own.

To execute the next milestone:

```
Execute the next unfinished milestone of plans/active/<plan>.md,
following plans/PLANS.md. One milestone, update the living sections,
commit, stop.
```

To author a plan:

```
Read AGENTS.md first. Then follow skills/plan-author/SKILL.md to author
a plan for <the goal, naming the debt row or commissioning decision and
the owner facts research cannot supply>. Stop at a committed plan.
```

To run the gardening sweep or a retrospective, name the procedure the same way:
`skills/doc-garden/SKILL.md` or `skills/retro/SKILL.md`.

### The lifecycle of a piece of work

```
idea or debt row
      │
      ▼
plan-author ──► plans/active/<plan>.md (committed, nothing implemented)
      │
      ▼  person reviews the plan
plan-execute ──► milestone 1 done, evidence recorded, committed, session ends
      │
      ▼  person reviews the diff and the plan's living sections
plan-execute ──► milestone 2 ... and so on
      │
      ▼
retro ──► lessons routed to their owning files
      │
      ▼
specs updated, decisions graduated, plan moved to plans/completed/
      │
      ▼
doc-garden (on its own cadence)
```

### Review between rounds

At L0, a person reads at plan boundaries: after a plan is written, and between
milestones. They read the diff the last session landed alongside the plan's
living sections. Their findings go into the plan's Decision Log as dated
entries with the reviewer named as author. The completed plans in
`plans/completed/` show this in practice.

### The watched loop

`tools/loop-runner` automates "start the next milestone session" while keeping
a person in charge. It lives in this repository and drives a plan in *another*
repository:

```sh
./tools/loop-runner run <repo-dir> --limit <n> [--plan <path>] [--logs <dir>] -- <harness command...>
```

For example, with the Claude Code invocation used in the recorded runs:

```sh
./tools/loop-runner run ~/code/my-project --limit 2 -- claude -p --model opus --dangerously-skip-permissions
```

That flag turns off Claude Code's permission prompts, which an unattended
session needs. Only aim it at a repository you're prepared to let an agent
change freely.

For each milestone it starts the harness command in the target directory. The
session gets no stdin, and its final argument is a short brief that names the
plan. When the session exits, the runner doesn't take its word for anything.
It reads git: the new commits, whether all of them modified the plan file, and
which `Progress` item flipped to done. Exit codes have been seen wrong in both
directions, so a zero exit proves nothing.

It halts on the first iteration that didn't clearly advance exactly one
milestone: nothing committed, more than one milestone ticked, a tick without a
timestamp, a commit that didn't touch the plan, or a nonzero exit. Before
starting, it refuses a plan with a missing living section or an undated
completed entry, because it couldn't tell inherited work from its own. When
it halts, it tells you which plan and milestone it stopped on and where the
session log is.

A halt is the tool working. Read the transcript, fix the cause, and run it
again; it picks up where it stopped. Exit 0 means it reached the limit or the
end of the plan, 1 means it halted, 2 means it refused to start. It refuses to
run against this repository.

The loop is "watched" by design: a person starts every run, picks the limit,
and reads the result. Decision record 0027 in `docs/decisions/` explains why
unattended runs were ruled out.

## Working on this repository

The first thing to know: a change here usually has to land in both halves. If
you improve a skeleton, the live file at the same path probably needs the
change too, and the reverse. `AGENTS.md` is the agent's entry point and applies to
people too.

### Setup

One line per clone, so that commits run the checks:

```sh
git config core.hooksPath tools/hooks
```

After that, the hook runs `./tools/verify` before each commit and blocks the
commit if it fails. You can still skip the hook with `--no-verify`, and a fresh
clone has no hook until you run that line. That gap is `D8` in `docs/DEBT.md`.

### The cheap command

```sh
./tools/verify
```

It runs seven checks in a fixed order, prints each one's output unchanged, and
exits nonzero if any fails or can't decide. The budget is five seconds; it
usually takes about three. A clean run ends with
`fast-verify: 7 of 7 checks passed (3s).`

| Check | What it decides | Card |
| --- | --- | --- |
| `scaffolding-markers` | No filled live file still has a `{{FILL` slot or `GUIDANCE` block | none (the marker syntax belongs to `template/AGENTS.md`) |
| `template-live-drift` | Every template file has a live counterpart, `plans/PLANS.md` is byte-identical in both, and skeleton headings appear in order in the live copy | `docs/capabilities/template-live-drift.md` |
| `doc-integrity` | Every backticked or linked path in the live docs resolves | `docs/capabilities/doc-integrity.md` |
| `prose-duplication` | No eight-word run appears in two files, across both halves | `docs/capabilities/prose-duplication.md` |
| `evidence-check` | Active plans have all four living sections, and ticked items have timestamps | `docs/capabilities/evidence-check.md` |
| `boundary-lint` | References follow the layer map's three decidable rules | `docs/capabilities/boundary-lint.md` |
| `loop-runner` | The loop driver still halts correctly, tested against five scratch repos with a stub harness | `docs/capabilities/loop-runner.md` |

Every failure prints a remediation message: what rule broke, the exact
offender, and what to do. Read it before anything else.

Some checks have allowlists in `tools/allow/`. Each entry is one line with a
reason, for the rare case where a dangling path or a shared sentence is
intentional. Entries that stop matching anything are counted as stale in the
passing output. The contract every check follows (exit codes, output shape,
allowlist format, how the time budget is set) is
[docs/specs/check-protocol.md](docs/specs/check-protocol.md).
[docs/specs/mechanical-checks.md](docs/specs/mechanical-checks.md) covers what
each check decides and what's still left to a person.

### Trials

```sh
./tools/blueprint-eval new <label>          # build a trial repo outside this tree
./tools/blueprint-eval check <repo-dir>     # inspect a filled trial copy
```

`check` answers three questions about a trial copy after its bootstrap: did
the procedures arrive in the layout every harness needs (`layout`), did any fill
slots or guidance blocks survive (`fill`), and do the copy's own paths resolve
inside the copy (`references`). You can pass one or more of those names to run
only some parts. The rest of a trial is a person starting an agent in the copy
and reading what it did. `./tools/verify` doesn't run either driver, because
their subjects (a trial copy, an agent session) aren't part of this repository.

### Changing things

- **Multi-file or multi-session work** needs an ExecPlan in `plans/active/`,
  written with `skills/plan-author/SKILL.md`.
- **Adding a check.** Start from a card, follow
  [docs/guide/implementing.md](docs/guide/implementing.md), put the script in
  `tools/checks/`, and add its name to the `CHECKS` list in `tools/verify`. The
  list is written out by hand, not globbed, so adding or removing a check always
  shows up in a diff. Joining the list also puts it behind the commit hook.
- **Editing a procedure in `skills/`.** Procedures travel to other projects, so
  a change is checked by re-running bootstrap trials in each harness. Per
  decision record 0025 in `docs/decisions/`, editing a procedure retires
  earlier trial results for the part that changed. Batch procedure edits where
  you can.
- **Recording a decision.** Log it in the plan's Decision Log first. If it
  still matters once the plan is done, it moves to `docs/decisions/` in the
  format of
  [docs/decisions/DECISION_FORMAT.md](docs/decisions/DECISION_FORMAT.md).

## What is proven and what isn't

What has been shown by running it:

- Copying `template/` into an empty folder with `cp -R` leaves no broken
  internal links, and the procedures reference only paths the copy provides.
- In each of Claude Code, Codex CLI, and omp, a fresh project was bootstrapped,
  given a plan, and had its first milestone executed with no access to this
  repository. Each ended with a small working command-line tool and a test
  command the project chose itself.
  [docs/specs/bootstrap-flow.md](docs/specs/bootstrap-flow.md) has the dates
  and details.
- Seven checks run on every commit here. Each of the six backed by a card
  passed the promotion bar on its way to `enforced`.

What hasn't:

- **Existing codebases.** Every trial was greenfield (`D6`), and every trial
  project was the same kind of small CLI tool.
- **A damaged procedure set.** Nobody has yet designed a breakage that a
  harness actually fails on, because sessions read the files directly and route
  around it. That's why `blueprint-eval` stays `specced` (`D13`), and
  decision record 0024 in `docs/decisions/` caps it at `built` in any case.
- **An unskippable gate.** The only gate is a local hook (`D8`). There's no CI
  job yet.
- **Autonomy above L0.** Nothing here runs unattended, and the loop runner
  claims no rung.

The full list of known gaps, each with the trigger that would end it, is
[docs/DEBT.md](docs/DEBT.md).

## Glossary

**Agent session.** One run of an agent harness, from start to exit. It starts
with no memory of earlier sessions.

**Harness.** The agent product that runs a session: Claude Code, Codex CLI,
omp, and so on.

**Payload.** The contents of `template/`. What a target project copies in.

**Live instantiation.** The filled-out payload at this repository's root.

**Skeleton.** A payload file with `{{FILL: ...}}` slots and `GUIDANCE` notes,
meant to be filled in and cleaned up.

**Format document.** A payload file kept verbatim that defines a shape:
`CARD_FORMAT.md`, `DECISION_FORMAT.md`, `PLANS.md`.

**Procedure** (or skill). One of the six `skills/<name>/SKILL.md` files an agent
reads and follows.

**Map.** The list in `AGENTS.md` that says which file owns which kind of fact.

**Entry-point shim.** A file such as `CLAUDE.md` that exists only to send a
harness to `AGENTS.md`.

**Reachability.** Whatever a bootstrap installs so a harness can find the
procedures on its own, such as a committed link. Optional, because reading the
file by path always works.

**ExecPlan** (or plan). A self-contained plan file under `plans/` that follows
`plans/PLANS.md`.

**Milestone.** One session's worth of a plan, with observable acceptance.

**Living sections.** `Progress`, `Surprises & Discoveries`, `Decision Log`,
`Outcomes & Retrospective`. Updated in every session.

**Evidence.** Output someone actually observed, recorded in the plan.

**Capability card.** A one-page spec for one mechanical check.

**Register.** The table in `docs/capabilities/index.md` holding each card's
status.

**Promotion bar.** The requirement to see a check fail on a real violation
before calling it `built`.

**Remediation message.** The text a check prints when it fails: rule, offender,
next action.

**Cheap verification command.** The one command a project publishes for agents
to run after every edit. Here, `./tools/verify`.

**Rung.** A level on the maturity ladder, L0 to L3.

**Debt row.** An entry in `docs/DEBT.md` with an ID like `D8`.

**Decision record.** A numbered file in `docs/decisions/` explaining a lasting
choice.

**Spec.** A file in `docs/specs/` describing current behavior.

**Reflection rule.** A finished plan updates a spec, or says its outcome was
internal.

**Trial.** A throwaway project built from the payload to test it in a real
harness.

**Watched loop.** `tools/loop-runner` driving milestone sessions under a limit a
person set, halting on anything unexpected.

**Provenance line.** An optional line in a project's `GOALS.md` recording where
its payload came from, so a future reader can find this guide.

## Where to read next

| If you want to... | Read |
| --- | --- |
| Get the mental model in more depth | [docs/guide/overview.md](docs/guide/overview.md) |
| Adopt the kit in a new project | [docs/guide/adopting.md](docs/guide/adopting.md) |
| Run a project that has it | [docs/guide/operating.md](docs/guide/operating.md) |
| Turn a card into a working check | [docs/guide/implementing.md](docs/guide/implementing.md) |
| Know why the kit exists and what it won't do | [GOALS.md](GOALS.md) |
| Understand the layer rules | [ARCHITECTURE.md](ARCHITECTURE.md) |
| Write or run a plan | [plans/PLANS.md](plans/PLANS.md) |
| See what's enforced today | [docs/capabilities/index.md](docs/capabilities/index.md) |
| See how much autonomy is allowed today | [docs/MATURITY.md](docs/MATURITY.md) |
| Find out why something is the way it is | [docs/decisions/](docs/decisions/) |
| Work in this repository as an agent | [AGENTS.md](AGENTS.md) |
