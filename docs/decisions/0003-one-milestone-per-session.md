# 0003 — One milestone per fresh-context session, then stop

## Decision

An implementing session reads the agent guide, one plan, and the files that
plan names; executes exactly one milestone; updates the plan's living
sections with observed evidence; and stops. Continuing into a second
milestone in the same session is prohibited. A milestone that will not fit
is split in place in the plan's Progress section, with the split recorded,
and the session exits. The loop that advances milestones lives outside the
session.

## Rationale

The plan file is durable context; the context window is a disposable cache.
A session that works through compaction silently breaks the guarantee that
the work is restartable from the plan alone, because the discoveries that
shaped its second half were summarized away instead of written down — and
the summary is not in the repository, so as far as the next agent is
concerned those discoveries never happened.

The alternative, long sessions covering several milestones, is cheaper per
milestone in tool calls and worse on the only axis that matters: whether the
next agent can pick up from the file. This also forces milestone sizing to
be honest at authoring time, since a milestone whose acceptance cannot be
stated as a few observable checks will not survive the stop rule.

## Date

2026-09-16, v1 blueprint design conversation.

## Status

accepted
