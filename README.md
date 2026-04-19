# shiraz-coding-style

A portable coding-agent skill for **Codex** and **Claude Code** that enforces:

- **strict TDD by default**
- **clarify before coding**
- **minimal, surgical diffs**
- **strong verification and explicit residual risk**
- **simplicity-first, anti-overengineering engineering discipline**

This skill is designed for implementation work: bug fixes, features, refactors, migrations, UI changes, and multi-step engineering tasks where correctness matters more than speed.

## What it does

`shiraz-coding-style` gives a coding agent a default operating methodology:

1. Clarify expected behavior and hidden product choices.
2. Resolve ambiguity from the repo, tests, docs, and existing behavior.
3. Ask only blocking clarifying questions.
4. Write **RED** tests first.
5. Confirm the tests fail for the correct reason.
6. Implement the **smallest possible change** to reach **GREEN**.
7. Refactor only while staying GREEN.
8. Review the final diff for regressions, overreach, and unnecessary complexity.

It combines:

- **TDD-first implementation discipline**
- **verification-centered development**
- **Karpathy-style simplicity**
- **surgical change management**
- **explicit handling of edge cases and assumptions**

## When to use it

Use this skill for:

- bug fixes
- new feature implementation
- refactors
- API changes
- UI changes
- migrations
- test creation or repair
- implementation-oriented code review
- multi-step engineering tasks

This skill is especially useful when you want the agent to:

- avoid guessing
- avoid overengineering
- keep diffs small and auditable
- make behavior explicit before coding
- prove correctness instead of asserting it

## When not to use it as the primary workflow

This should not be the main workflow for:

- pure code reading with no implementation
- architecture brainstorming with no concrete change yet
- incident triage with no code change yet
- one-off research tasks
- documentation-only tasks with no behavior change

## Core philosophy

### 1. Clarify before coding
Do not let the agent silently pick among multiple plausible behaviors.

### 2. Tests before implementation
For implementation tasks, the default rule is:

> no implementation before RED tests

### 3. Minimal code to green
The goal is not “more code.” The goal is the smallest defensible change that satisfies the tests and the requested behavior.

### 4. Simplicity beats speculation
Do not add abstraction, configuration, edge-case handling, or future-proofing without a present need.

### 5. Verification over confidence
The agent must report what was actually verified, what passed, and what remains unverified.

## The default workflow

For implementation work, the required order is:

```text
clarify → edge cases → RED tests → confirm RED → minimal implementation → GREEN → refactor → review
```

Expanded form:

1. Clarify behavior and acceptance criteria.
2. Enumerate important edge cases and assumptions.
3. Resolve ambiguities from the repo and current behavior.
4. Ask only blocking questions.
5. Write failing tests first.
6. Confirm the tests fail for the intended reason.
7. Implement the minimal code required to make them pass.
8. Run the relevant verification checks until GREEN.
9. Refactor only while preserving GREEN.
10. Review the final diff for correctness, regressions, and unnecessary complexity.

## Edge cases the skill pushes the agent to inspect

Depending on the task, the skill prompts the agent to think through:

- input validation and malformed data
- null / empty / duplicate cases
- retry and stale-state behavior
- partial failure and rollback behavior
- concurrency and idempotency
- compatibility and migration boundaries
- auth, authz, injection, unsafe defaults, and data loss risks

It also explicitly discourages adding tests for unrealistic scenarios just to look comprehensive.

## What makes this different

Many coding-agent prompts say “be careful” or “write good code.”
This skill is more opinionated and operational:

- it requires **decision-grade clarification**
- it requires **RED before code**
- it requires **minimal local changes**
- it discourages **cleanup tangents and speculative abstractions**
- it emphasizes **behavioral verification over plausible reasoning**
- it encourages **fresh-context review** for important changes

## Repository contents

```text
shiraz-coding-style/
├── SKILL.md
└── README.md
```

## Installation

### Codex

Place this folder under a Codex skill directory, commonly inside your repo at:

```text
.agents/skills/shiraz-coding-style/
  SKILL.md
```

A repo-level placement is usually the most practical because the skill travels with the codebase.

### Claude Code

Place this folder at:

```text
.claude/skills/shiraz-coding-style/
  SKILL.md
```

If you are packaging it for upload or sharing, keep the folder name aligned with the skill name.

## How to use it

### Let the tool auto-load it

Prompts that strongly match the skill’s intent are the most likely to trigger it implicitly.
Examples:

- “Fix this bug using strict TDD.”
- “Clarify edge cases first, then write failing tests, then implement the minimal fix.”
- “Use RED-GREEN-refactor and keep the diff surgical.”
- “Refactor this with the simplest solution that works and no speculative abstraction.”

### Invoke it explicitly

Examples:

#### Codex

```text
Use shiraz-coding-style to fix this auth refresh bug.
```

#### Claude Code

```text
/shiraz-coding-style Fix this auth refresh bug
```

## Example prompts

### Bug fix

```text
Fix the session refresh bug using strict TDD.
Clarify any blocking ambiguities first.
Write RED regression tests, confirm they fail for the right reason, then implement the minimal fix and keep the diff surgical.
```

### Feature

```text
Build this API endpoint with tests first.
Resolve ambiguities from the repo before asking questions.
Use RED-GREEN-refactor, avoid speculative abstractions, and keep the implementation as small as possible.
```

### Refactor

```text
Refactor this module with characterization tests first.
Keep the design simple, preserve behavior, and avoid adding new abstraction unless the current code demands it.
```

## Expected outcomes

When this skill is working well, the agent should:

- ask better questions
- guess less
- write better tests
- make smaller diffs
- avoid speculative design
- verify more rigorously
- produce code that is easier to review and trust

## Intended audience

This skill is for:

- engineers who want more disciplined coding-agent behavior
- teams standardizing how agents implement changes
- repositories where correctness and auditability matter
- people who prefer simple, explicit, test-driven engineering over cleverness

## Related repo-level guidance

This skill can be used by itself.

If you want repo-specific commands, architectural constraints, and always-on local rules, add those separately in files such as:

- `AGENTS.md` for Codex-oriented repo instructions
- `CLAUDE.md` for Claude Code project memory

The skill handles the reusable workflow. Repo instruction files handle local project rules.

## License

MIT license.

