# 0011 — The decisions directory has no index

## Decision

Decision records are discovered by their `NNNN-short-slug.md` filenames.
This directory gets no index file.

## Rationale

The other two index files in the docs tree exist because each owns something
its sibling files cannot: `docs/capabilities/index.md` owns mutable status,
`docs/specs/index.md` owns behavior routing. A decision record is immutable
once written and its filename is its summary, so an index here would carry
no fact of its own and exactly one guaranteed drift surface — the record
that lands without its index row.

## Date

2026-09-17, settled while writing the payload's docs tree.

## Status

accepted
