# doc-integrity

## Invariant

Every path reference in this project's live artifact half resolves to a file
or directory that exists. A reference is a path written in backticks or used as
a markdown link target that contains a slash and none of space, `*`, angle
bracket, or brace. Those exclusions are what keep the check from reporting
things that were never references: a command quoted whole, a glob such as
`skills/*/SKILL.md`, and a placeholder-bearing path such as
`skills/<name>/SKILL.md`. A bare filename with no directory is not a path
reference either, and is not in the set. Neither is a path that begins with a
slash: it names a location on the machine rather than one in this repository —
a principle stating where a tool writes its scratch files names /tmp that way
— and testing it from the repository root asks a question nobody wrote. It is
excluded at extraction rather than allowlisted, because the allowlist is for
judgements and this one is decidable from the first character.

The checked set is the live artifact half: `AGENTS.md`, `GOALS.md`,
`ARCHITECTURE.md`, `CLAUDE.md`, everything under `docs/`, `plans/PLANS.md`,
and everything under `skills/`.

Four classes of non-resolving reference are legitimate and must not be
reported, and all four are decidable:

- a sibling filename cited from inside the directory that owns it, resolved
  relative to the citing file rather than to the repository root;
- an illustrative filename in a format document, such as the decision-record
  name pattern — files whose own name ends in `_FORMAT.md` are skipped whole;
- quoted material: a reference inside a fenced or indented block is a
  transcript, not a citation. A capability card's remediation example names
  the file its own failing case creates, so a card that is checked as prose
  reports itself;
- a deliberate mention of a file that does not exist or must not exist, which
  is a judgement no checker can make. Those are carried in
  `tools/allow/doc-integrity.txt` as exact `citing-path:reference` keys, each
  with a one-line reason. An allowlist entry is cheap to review; a checker
  that reports legitimate references is switched off within a week, and then
  nothing is checked at all.

Two parts of the tree are outside the checked set, each for a reason that is
not convenience. Plan files under `plans/active/` and `plans/completed/` are
out, because a plan names the files it will create before they exist and the
files it deleted after they are gone: the plans in this repository hold
hundreds of references that resolve to nothing and not one of them is a
defect. The payload half under `template/` is out, because a reference
written there must resolve inside the copy a target project receives rather
than from this repository's root. That is a different question with a
different answer, and it belongs to `docs/capabilities/boundary-lint.md`,
whose `no-outward-payload-reference` rule decides it.

Two things this card deliberately does not decide. Whether a document's
content is still true — freshness — is not decidable from text and stays a
review concern. Whether a plan carries its required sections belongs to
`docs/capabilities/evidence-check.md`, because plans have their own
structural contract and their own remediation.

## Enforcement point

`./tools/verify` from the repository root, and the versioned hook
`tools/hooks/pre-commit`, which runs that same command and refuses the commit
when it exits nonzero. There is no continuous integration to name: nothing runs
on the remote this repository pushes to, and the gate's skippability is
`docs/DEBT.md` `D8`.

## Acceptance

Passing case: on a clean checkout the check exits zero and prints one summary
line stating how many references it resolved, across how many artifacts, how
many format documents it skipped, and how many allowlist entries it applied
and how many of them are stale. The reference count must be greater than zero,
because a run that prints nothing is indistinguishable from a run that checked
nothing, and a wrong path or an over-eager filter passes forever.

The stale count is part of the summary rather than a failure. An entry that
matches nothing means the mention it excused was fixed or deleted, which is
good news the check has no business refusing — but an allowlist whose entries
are never re-read grows into the thing it was meant to bound.

Failing case: add one map line to `AGENTS.md` citing a path under docs/ that
does not exist — a hyphen, a space, the backticked path, an em dash, and the
word placeholder, the shape the remediation block below shows — then run
`./tools/verify` and observe a nonzero exit whose message names the citing
file, its line number, the unresolved target, and the three possible next
actions. Delete the line and observe the check pass again. Do this before
marking the card `built`.

## Remediation message

    doc-integrity: AGENTS.md:47 references `docs/NOPE.md`, which does not
    exist. Create the file, correct the path, or — if the mention is
    deliberate — add `AGENTS.md:docs/NOPE.md` to
    tools/allow/doc-integrity.txt with a reason.

The message names the citing file and line, the unresolved target, and the
three possible next actions. A message that says only "broken link found"
forces the reader to re-derive everything the checker already knew.

## Per-stack hints

No toolchain applies: `ARCHITECTURE.md` states that every artifact in this
repository is markdown, operated on with shell and git only. One `awk` pass
extracts every backticked span and link target from the checked set, and
`test -e` decides each one from the repository root and then from the citing
file's directory. Link checkers built for published sites
(`lychee`, `markdown-link-check`) would miss most of what matters here,
because nearly every reference in these artifacts is a backticked path rather
than markdown link syntax.
