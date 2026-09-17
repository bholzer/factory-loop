# 0004 — Template files come in two classes, distinguished by markers

## Decision

`{{FILL: ...}}` marks a content slot to be replaced. Every HTML comment
block in a template file begins with the word GUIDANCE and is authoring
instruction to be deleted once the file is filled. Files carrying either
marker are skeletons and are filled on instantiation; files carrying neither
are shipped verbatim and kept as-is by a target project. A skeleton is fully
filled when a search for both markers comes back empty.

## Rationale

A reader must be able to tell a placeholder from an instruction at a glance,
and "is this template filled in?" has to be answerable by a command rather
than by judgement. Both markers are greppable, which is what makes the fill
check mechanical.

The two-class split exists because format-defining files are never filled at
all. The first drafts of the capability-card and decision-record format
documents used the skeleton convention, so they failed the fill check
permanently — markers on files that a target project keeps verbatim forever.
The alternative was an exception list, which would then have had to be
carried by every procedure that fills, gardens, or drift-checks the payload:
three copies of one carve-out. Prose in those two files costs nothing and
keeps the fill check total. Guidance lives in comments rather than visible
text so a filled artifact renders clean, and is deleted anyway because
authoring scaffolding in a shipped project file is a tax on every later
session.

## Date

2026-09-17, settled while writing the payload's docs tree.

## Status

accepted
