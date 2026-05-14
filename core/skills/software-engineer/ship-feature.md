# Skill: ship-feature

**Type:** composite
**Sub-skills:** spec-writing → implementation → code-review

## Purpose

End-to-end workflow for delivering a feature: from an ambiguous requirement to merged, production-ready code. Follows a preparation, execution, and verification loop using spec writing, implementation, and review.

## When to Invoke

- A new feature or significant change needs to go from requirement to merged PR.
- The requirement exists but is not yet ready for an AI agent to implement.

## Workflow

### Step 1 — Preparation (@spec-writing)

Before any code is written:

1. Clarify the requirement. If the input is ambiguous, resolve ambiguity now — not in a review comment after the code is written.
2. Produce a spec using the spec-writing skill. Minimum: problem statement, scope, GIVEN/WHEN/THEN cases, interface contract.
3. Get sign-off from the relevant stakeholder or technical lead on the spec. This is the last natural checkpoint before work begins.

**Gate:** Do not proceed to implementation until the spec has a defined scope and at least one open question is not blocking.

### Step 2 — Execution

Hand the spec to an AI agent or implement directly:

- If using an AI agent: pass the spec as the prompt. Be explicit about what's in scope. Include the relevant portions of coding-standards and security instructions.
- If implementing directly: follow the spec case-by-case. Don't add cases not in the spec.

Track implementation against the GIVEN/WHEN/THEN cases in the spec. Each case should have a corresponding test.

**Gate:** All specified cases have passing tests before moving to review.

### Step 3 — Review (@code-review or @agent-output-review)

- If the implementation was AI-generated: use the agent-output-review skill.
- If human-authored: use the code-review skill.

No code merges without passing this review. Blocking issues from the review go back to Step 2.

### Step 4 — Verification

Before marking the feature done:
- [ ] All GIVEN/WHEN/THEN cases from the spec have passing tests.
- [ ] The code-review or agent-output-review returned no blocking issues.
- [ ] The change has been exercised on a non-production environment (staging, dev, local).
- [ ] Any open questions from the spec have been resolved and documented.

## Output

- Completed spec document.
- Merged PR with passing tests.
- A summary of any spec cases that were descoped during implementation and why.
