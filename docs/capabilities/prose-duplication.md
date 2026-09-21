# prose-duplication

## Invariant

No run of eight consecutive words appears in two of this project's markdown
artifacts. Stated so a violation is decidable: strip quoted material, normalize
each artifact to its sequence of lowercase alphanumeric words, take every
window of eight consecutive words, and require the window sets of any two
artifacts to be disjoint.

Eight words is the floor, not a preference. Shorter windows fire on ordinary
English connective phrasing, and a check whose findings are mostly noise is
switched off; longer windows miss a restated one-sentence rule, which is the
duplication that actually goes stale.

The checked set is both halves — `AGENTS.md`, `GOALS.md`, `ARCHITECTURE.md`,
`CLAUDE.md`, everything under `docs/`, `plans/PLANS.md`, everything under
`skills/`, and everything under `template/`. Two parts of the tree are outside
it. Records under `docs/decisions/` each restate the rule they installed, and
`docs/decisions/DECISION_FORMAT.md` forbids replacing a record's text with a
pointer, so every record's copy is historical by construction; the format
document itself stays in the set, because it is half of the format-document
pair below. Plan files under `plans/active/` and `plans/completed/` quote
convention text, command transcripts, and the artifacts they change, and that
quoting is what makes a plan self-contained.

Six classes of shared window are legitimate and must not be reported. Five are
decidable and one is a judgement.

- quoted material: fenced blocks, indented blocks, and HTML comment blocks are
  stripped before windowing. An indented block reproduces a remediation
  message or a transcript verbatim, and an HTML comment in the payload is
  authoring guidance that a target project deletes when it fills the file, so
  two skeletons still carrying the same guidance block are not duplicating
  each other;
- a pair at the same relative path across the two halves: a file under
  `template/` against the live file whose path below the repository root is
  the same. `ARCHITECTURE.md` declares that correspondence, and a live card
  sitting at a payload card's relative path is that card's instantiation, so
  the pair is a copy by construction and that rule owns it;
- a pair whose files are governed by the same sibling `*_FORMAT.md` document,
  decidable by comparing the format document found beside each file and
  skipping the pair when both names match. Their shared text is the format's,
  not either file's;
- a pair of format documents themselves, decidable by skipping pairs where
  both filenames end in `_FORMAT.md`;
- an attributed restatement — the section the window starts in, in either
  file, cites the other file in backticks, which is what `docs/PRINCIPLES.md`
  requires of a restatement that has to exist. A citation names the other file
  four ways: as a repository-relative path, as a bare sibling filename, — from
  inside `template/`, whose references resolve inside the copy a target project
  receives — as a path relative to `template/`, and as either copy of a fact
  whose owner sits at the same relative path in both halves. Without the third
  form a payload file attributing a fact to `AGENTS.md` would read as
  unattributed here while its live twin reads as attributed; without the fourth
  the diagonal goes unattributed instead — a live file citing the live owner
  still collides with the payload copy of that owner, which holds the same
  sentence by construction;
- a window that is a proper-noun run, a path list, or a section-name list both
  files must spell out to name the same thing. This is the judgement class,
  carried in `tools/allow/prose-duplication.txt` as exact `fileA:fileB:window`
  keys with the two paths in ascending order, each with a one-line reason. One
  entry per window rather than per pair: a pair-level exception would excuse
  duplication nobody has read yet.

When a payload card is instantiated, every allowlist entry naming it gains a
twin naming the live copy. `docs/decisions/0016-skill-and-card-may-restate-one-fact.md`
records two procedure-and-card pairs that cannot be repaired by a pointer in
either direction; instantiating one of those cards duplicates its pair against
the live copy, and the twin carries the same reason.

Skipping same-format siblings means duplication between two cards, or between
two records, is not decided here. That is the price of not reporting the
format's own scaffolding on every pair; `CARD_FORMAT.md` in this directory
states the one-invariant-per-card rule that keeps two cards from covering the
same ground, and it is enforced by review.

What this card deliberately does not decide: which copy is canonical. The check
reports a pair; a reader decides which file owns the fact and which one gets a
pointer instead. That is why no rung in `docs/MATURITY.md` gates on this card —
it removes the searching, not the routing.

## Enforcement point

`./tools/verify` from the repository root, and the versioned hook
`tools/hooks/pre-commit`, which runs that same command and refuses the commit
when it exits nonzero. There is no continuous integration to name: nothing runs
on the remote this repository pushes to, and the gate's skippability is
`docs/DEBT.md` `D8`.

## Acceptance

Passing case: on a clean checkout the check exits zero and prints one summary
line stating how many artifacts it compared, how many eight-word windows it
examined, how many allowlist entries it applied, and how many of them are
stale. The first three counts must be greater than zero — a glob that matches
nothing, or a filter that strips every line, passes forever.

The stale count is part of the summary rather than a failure, for the reason
`doc-integrity` states about its own: an entry that matches nothing means the
duplication it excused is gone, which the check has no business refusing, but
an allowlist nobody re-reads grows into the thing it was meant to bound.

Failing case: copy one whole sentence out of `docs/PRINCIPLES.md` into
`AGENTS.md` under Working rules, run `./tools/verify`, and observe a nonzero
exit with the message below naming both files, the shared window, and the four
possible next actions. Delete the copied sentence and observe the check pass
again. Do this before marking the card `built`. The destination section
matters: a section of `AGENTS.md` that cites `docs/PRINCIPLES.md` makes the
copy an attributed restatement, which is a legitimate class rather than a
failure to demonstrate.

## Remediation message

    prose-duplication: AGENTS.md and docs/PRINCIPLES.md share the window
    "the rule every fact lives in exactly one" (24 words shared in total).
    One of the two owns this fact. Delete the copy, leave a reference to the
    owning file, name the owning file in the same section so the restatement
    reads as attributed, or — if the window is a proper-noun run, a path list
    or a section-name list both files must spell out — add
    "AGENTS.md:docs/PRINCIPLES.md:the rule every fact lives in exactly one"
    to tools/allow/prose-duplication.txt with a reason.

The message names both files, the actual shared text, how much of the first
file the overlap covers, and the four possible next actions including the exact
allowlist key to add. "Duplicate content found" fails this bar: the reader has
to re-derive the window the checker already computed.

## Per-stack hints

No toolchain applies: `ARCHITECTURE.md` states that every artifact in this
repository is markdown, operated on with shell and git only. One `awk` pass
maps each window to the files holding it and decides every pair at once, which
is what keeps the run under a second over twenty-seven thousand windows; the
pairwise-intersection shape computes the same answer in several hundred process
invocations. The same pass records, per file and per section, the backticked
filenames that section cites, because the attribution class needs them.
