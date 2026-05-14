# Skill: code-review

**Type:** atomic
**Scope:** Review human-authored code changes against coding standards, correctness, and security.

## When to Invoke

- A pull request or diff is ready for review before merge.
- Asked to review a specific file, function, or change set.

## Inputs

- The diff or file(s) to review.
- The stated purpose of the change (ticket, PR description, or verbal summary).

## Procedure

1. **Understand intent first.** Read the PR description or ask for one before reading code. Reviewing without knowing the goal produces noise.

2. **Correctness** — does the code do what it claims?
   - Trace the happy path end-to-end.
   - Identify edge cases the code doesn't handle. Note whether they are in scope.
   - Check for off-by-one errors, null/undefined access, incorrect assumptions about input ranges.

3. **Standards compliance** — does the code match the codebase's conventions?
   - Naming, formatting, file organisation.
   - No speculative abstractions or features beyond the stated scope.
   - Comments only where the why is non-obvious.

4. **Security** — does the code introduce vulnerabilities?
   - User input reaching a sink without sanitisation.
   - Secrets, PII, or internal detail leaking to logs or error responses.
   - Authorisation checks missing or bypassable.

5. **Tests** — do they verify intent?
   - Every changed behaviour should have a corresponding test.
   - Tests should fail when the business logic they protect is removed.
   - Missing tests are a blocking concern, not a suggestion.

6. **Surgical check** — does the diff touch only what the task requires?
   - Flag unrelated changes. They should be split out, not bundled.

## Output

A structured review with three sections:

**Blocking** — must be resolved before merge. Each item: file:line, what's wrong, why it matters, suggested fix.

**Non-blocking** — improvements worth considering but not required. One line each.

**Confirmed** — two or three things done well. Keeps the signal-to-noise ratio honest.

If there are no blocking issues, say so explicitly.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "It works, so it's fine" | Correctness is one axis. A review also checks maintainability, security, and test coverage — all of which can fail even when the feature works. |
| "I'll approve with a comment" | Non-blocking comments on blocking issues let bad code merge. Classify honestly: if it must change, it's blocking. |
| "Tests can come later" | Missing tests are a blocking concern. Code that ships without tests creates debt that almost never gets paid. |
| "I don't want to seem harsh" | A review that avoids calling out real problems is not a review — it's approval theater. Specific, actionable feedback is respectful, not harsh. |
| "The diff is too large to review properly" | Then say so. A review of a diff too large to understand is worse than no review — it provides false assurance. Request a split. |

## Red Flags

- Approving without reading every changed file
- Commenting on style while missing a security vulnerability
- Flagging the same category of issue once but not consistently (reviewer bias rather than systematic check)
- Missing tests treated as non-blocking
- PR description absent and reviewer didn't ask for one before reviewing
- No "Confirmed" section — a review with only criticism misses the signal-to-noise calibration

## Verification

Before submitting a review:

- [ ] PR description was read (or requested) before the diff was opened
- [ ] Happy path traced end-to-end through the changed code
- [ ] At least two edge cases explicitly considered
- [ ] Security axes checked: input handling, auth, secrets, error messages
- [ ] Every changed behaviour has a corresponding test — or a blocking comment is filed
- [ ] Each blocking item has: file:line, what's wrong, why it matters, suggested fix
- [ ] "Confirmed" section lists at least one thing done well
