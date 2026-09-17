# 0012 — Evidence lives inside plans; there is no separate build log

## Decision

Observed evidence — the command that was run and what it printed — is
recorded in the plan that produced it, under Progress and Surprises &
Discoveries. No standalone build log, session journal, or context file
exists alongside plans.

## Rationale

One source of truth per unit of work. A separate log gives every observation
two plausible homes, so a reader has to check both and a writer has to
choose, and the choice is made differently by each session. The five-file
split considered at design time — goals, prompts, build plan, context, log —
achieved its ownership hygiene through file boundaries; at this scale the
same hygiene is reachable as rules inside one convention, and the extra
files were pure duplication surface.

A separate log earns its cost when plans grow too large to hold their own
evidence. That is a promotion to make deliberately, with the trigger
observed, rather than a structure to adopt in advance.

## Date

2026-09-16, v1 blueprint design conversation.

## Status

accepted
