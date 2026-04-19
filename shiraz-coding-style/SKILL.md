---
name: shiraz-coding-style
description: Use for software implementation, bug fixes, features, refactors, and reviews that should follow strict TDD, minimal diffs, strong verification, and simplicity-first engineering.
---

# shiraz-coding-style

## Purpose

This skill defines a default operating methodology for coding agents working on implementation tasks. Use it to maximize correctness, reduce guessing, keep diffs small and auditable, and ensure behavior is specified and verified before code is written.

This is a **TDD-first, verification-centered, simplicity-first** workflow.

## Use this skill when

Use this skill for:

- bug fixes
- new feature implementation
- refactors
- API changes
- UI changes
- migrations
- test creation or repair
- code review tied to intended code changes
- multi-step engineering tasks that require exploration, planning, implementation, and verification

Do not use this skill as the primary workflow for:

- purely exploratory code reading
- architecture brainstorming with no implementation yet
- incident triage with no code changes yet
- one-off research tasks
- documentation-only tasks with no behavior change

For implementation tasks, this skill is the default operating model.

## Core principles

1. **Clarify before coding.** Do not guess silently. Resolve ambiguity from the codebase, tests, docs, specs, and existing behavior before asking questions.
2. **Ask only blocking questions.** When ambiguity remains material, ask decision-grade questions instead of vague ones.
3. **Use TDD by default.** Start implementation by writing RED tests, then make them pass with the smallest viable change.
4. **Prefer simplicity.** No speculative abstractions, no extra configurability, no cleanup tangents, no future-proofing without a present need.
5. **Make surgical changes.** Every changed line should trace to requested behavior, required verification, or cleanup made necessary by the change itself.
6. **Verify, do not merely claim.** Report what was actually tested, what passed, and what remains unverified.
7. **Respect repo instructions.** Follow project-specific guidance in files such as `AGENTS.md`, `CLAUDE.md`, test docs, architecture notes, or local conventions. More specific repo instructions take precedence unless they conflict with the user request.

## Required implementation workflow

For implementation tasks, follow this order:

1. Clarify behavior and success criteria.
2. Enumerate important edge cases and assumptions.
3. Resolve ambiguities from the repo and existing behavior.
4. Ask only blocking clarifying questions if needed.
5. Write RED tests first.
6. Run the tests and confirm they fail for the correct reason.
7. Implement the minimal code needed to make them pass.
8. Run the relevant checks until GREEN.
9. Refactor only while staying GREEN.
10. Review the final diff for regressions, missing cases, security risks, and unnecessary complexity.

**Do not write implementation code before RED tests unless the task is explicitly exploratory or non-behavioral.**

## Task contract

Before implementing, make the task concrete using this contract:

### Goal
What should change?

### Context
Relevant files, logs, screenshots, issue IDs, reproduction steps, examples, links, and architectural boundaries.

### Constraints
Compatibility requirements, public API constraints, security requirements, performance limits, style rules, and areas that must not be touched.

### Done when
Exact observable completion criteria.

### Verification
Commands, tests, screenshots, manual checks, or benchmarks that will prove correctness.

### Output
What the final report should include, such as root cause, files changed, tests added, commands run, assumptions, and residual risks.

If the user prompt does not provide all of this, infer as much as possible from the repo before proceeding.

## Clarification protocol

Before writing tests or code:

- identify ambiguities, hidden product choices, and edge cases
- resolve what you can from code, tests, docs, tickets, and current behavior
- state assumptions explicitly
- present multiple reasonable interpretations instead of silently choosing one
- if a simpler approach solves the real problem, say so
- push back when the requested approach is clearly overcomplicated relative to the goal

Ask questions only when the remaining ambiguity is material and blocking.

### Good clarifying questions

Good questions convert ambiguity into explicit product choices. Example:

- If duplicate emails appear in the CSV, should the whole import fail or should duplicates be deduplicated?
- If one row is invalid, should the entire request fail or should valid rows still be accepted?
- Should matching be case-sensitive or case-insensitive?
- Should existing records be updated, skipped, or rejected?

Avoid vague questions like “Any corner cases?”

## Edge-case checklist

Before writing tests, inspect the categories that materially matter for the task and state which will be covered now versus deferred:

- **Input space:** empty, null, malformed, out-of-range, duplicate, whitespace, encoding, case sensitivity
- **State and lifecycle:** first run, retry, stale state, partially completed state, already exists, deleted/archived/disabled states
- **Failure behavior:** timeout, dependency unavailable, invalid configuration, permission denied, partial write, rollback behavior
- **Concurrency:** race conditions, duplicate submission, parallel updates, idempotency, locking behavior
- **Compatibility:** existing clients, backward/forward compatibility, schema boundaries, migration boundaries
- **Security and correctness:** authn/authz, tenant isolation, injection, escaping, secret leakage, unsafe defaults, data loss

Do not add tests or code for unrealistic edge cases merely to look comprehensive. Cover the realistic, risk-bearing cases.

## TDD rules

### RED

Write tests that describe the desired behavior before changing implementation.

RED must fail for the intended reason.

Bad RED:
- imports are broken
- the environment is misconfigured
- setup is incomplete

Good RED:
- the current implementation returns the wrong result
- the desired behavior is not implemented yet
- the regression still reproduces

When RED is established, state:

- which tests were added or changed
- why they fail now
- why that failure proves the desired behavior is not yet implemented

### GREEN

Implement the smallest possible change that makes the tests pass.

Rules:

- prefer minimal, local changes
- preserve existing patterns unless there is a strong reason not to
- reuse existing helpers before creating new abstractions
- do not silently change adjacent behavior
- do not broaden scope once the first failing test exists
- do not add dependencies unless justified

Default posture: **minimal code to green**.

### REFACTOR

Only refactor after tests are GREEN.

Refactor to:

- improve clarity
- reduce duplication
- align with existing patterns
- lower incidental complexity

Constraints:

- keep all relevant tests green
- if refactoring risks behavior change, stop and add more tests first
- do not use refactoring as a pretext for unrelated architecture changes

Sequence: **RED → GREEN → REFACTOR**

## Workflow by task type

### Bug fix

1. Reproduce the bug with a failing regression test.
2. Add nearby edge-case tests where useful.
3. Confirm RED.
4. Implement the smallest fix.
5. Run the bug test and relevant nearby tests.
6. Summarize root cause and why the tests now prevent recurrence.

### New feature

1. Identify acceptance criteria.
2. Convert them into failing tests first.
3. Include happy path plus the most important boundaries and error cases.
4. Confirm RED.
5. Implement only enough to reach GREEN.
6. Refactor while preserving GREEN.

### Refactor

Use characterization-first TDD.

1. Identify the current observable behavior.
2. Add tests that lock that behavior down.
3. Confirm those tests pass before refactoring.
4. Refactor in small increments.
5. Keep the suite green after each step.

### Migration

1. Define expected before/after behavior.
2. Write tests around compatibility and migration correctness.
3. Validate representative cases first.
4. Implement the migration logic minimally.
5. Verify forward behavior and rollback behavior when applicable.

### UI change

1. Define observable behavior and target visuals.
2. Add or update component, browser, snapshot, or integration tests where appropriate.
3. Confirm RED if behavior is changing.
4. Implement the minimal UI change.
5. Verify with screenshots or browser checks in addition to automated tests.

## Verification requirements

Never claim completion without saying what was actually verified.

Choose verification appropriate to the task:

- **Bug fix:** failing-before/passing-after regression test
- **Refactor:** behavior-preserving tests remain green
- **UI change:** browser check, screenshot comparison, or user-flow validation
- **API change:** contract tests or request/response assertions
- **Migration:** representative migrated records, dry run, or rollback check
- **Performance work:** before/after measurement

Verification rules:

- identify the smallest reliable verification command before editing
- run the relevant checks after implementation
- report exactly what passed
- explicitly mention what was not verified
- never substitute plausible reasoning for actual verification

## Review checklist

Before declaring completion, review the diff and answer:

- Does the code satisfy the requested behavior?
- Were the right tests added?
- Do the tests prove the behavior rather than mirror the implementation?
- Is the diff broader than necessary?
- Did we accidentally change adjacent behavior?
- Are there missing edge cases?
- Is compatibility preserved?
- Are there security, privacy, or data-loss risks?
- Is the result more complex than a strong senior engineer would consider necessary?

For important changes, prefer a fresh-context review pass or reviewer agent.

## Minimal-diff rule

Prefer small, reversible, auditable diffs.

Good task sizes:

- fix one bug
- add one endpoint
- refactor one module
- migrate a few representative files first
- add one component using existing patterns

Bad task sizes:

- rewrite the app
- clean up the codebase
- modernize everything
- improve performance everywhere
- make the UI better

When a task is large, reduce it into explicit milestones and validate a representative slice before scaling.

## Context and role management

Use context deliberately.

- use a fresh session for unrelated tasks
- if the agent fails repeatedly in the same way, stop, summarize, and restart with a sharper prompt
- maintain a written plan for long tasks
- separate roles when useful: researcher, planner, tester, implementer, reviewer
- use fresh-context review for important changes because the writer is biased by its own assumptions

For larger work, write a self-contained plan that another fresh agent could continue without prior chat context.

## Safety and permissions

Use tools intentionally.

Usually fine:
- read files
- search files
- edit repo files
- run tests, lint, and typecheck

Usually require explicit approval:
- package installation
- network access
- database modification
- migrations
- file deletion
- commits or pushes
- deployment
- secret or env-file access
- destructive git commands

Hard constraints:

- do not use destructive git commands unless explicitly authorized
- do not modify generated files directly unless the repo explicitly does so
- do not run risky operations merely because they are available

## Common failure modes

- **Edits too much:** shrink scope, constrain files, require a smaller milestone.
- **Invents architecture:** point to existing patterns and relevant files.
- **Claims done without proof:** require tests, commands, screenshots, or benchmarks.
- **Loops on the same failure:** stop, summarize, clear context, and restart sharply.
- **Writes shallow tests:** strengthen acceptance criteria and edge cases.
- **Overengineers:** simplify aggressively and remove speculative flexibility.

## Output format

Unless the user asks otherwise, end implementation work with a concise report containing:

1. what changed
2. tests or checks added
3. commands run
4. assumptions made
5. what remains unverified
6. residual risks or sensible follow-up work
