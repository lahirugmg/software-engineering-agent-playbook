# Skill: debugging

**Type:** atomic
**Scope:** Diagnose the root cause of a specific, reported defect and produce a verified fix.

## When to Invoke

- A specific bug has been reported with reproducible symptoms.
- A test is failing and the cause is unknown.
- Unexpected behaviour is observed in a known code path.

## Inputs

- Bug description: observed behaviour, expected behaviour, steps to reproduce.
- Relevant code, logs, stack traces, or error messages.
- Environment context if relevant (OS, runtime version, config).

## Procedure

1. **Reproduce first.** If you can't reproduce the bug, you can't confirm the fix. Write a failing test or reproduce it locally before touching production code.

2. **Form a hypothesis.** Based on the symptoms and code, state what you think is wrong and why. Write it down. Debugging without a hypothesis is guessing.

3. **Narrow the search space.**
   - Bisect: does the bug exist at commit X? At input Y? With config Z?
   - Use logging or assertions to confirm assumptions at each layer.
   - Trust nothing — verify that the code you think is running is actually running.

4. **Find the root cause, not just the symptom.**
   - A fix that suppresses the symptom without addressing the cause will recur.
   - Ask "why did this fail?" at least three times before settling on a cause.

5. **Write the fix surgically.** Change only what is necessary to address the root cause. Avoid refactoring surrounding code in the same commit.

6. **Verify the fix.**
   - The original reproducing test/case must now pass.
   - Existing tests must not regress.
   - If no test existed, add one that would have caught this bug.

7. **Explain the root cause.** One paragraph: what was wrong, why it was wrong, and why the fix is correct. This goes in the commit message or PR description, not the code.

## Output

- Root cause statement.
- The fix (diff or changed files).
- A test that confirms the fix and would catch a regression.
- Any related risks or follow-up items uncovered during investigation.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I know what's wrong, I'll skip reproduction" | Without a reproduction, you can't confirm the fix. You may fix the wrong thing and declare success. |
| "I'll fix it and add a test later" | Later rarely happens. The test is part of the fix — it proves the fix works and prevents recurrence. |
| "It's an obvious fix, I don't need a hypothesis" | Debugging without a written hypothesis lets you drift from one change to the next. Write it down — even obvious ones. |
| "The bug is in the code I just wrote" | It might be. But verify: the bug might be in code that was already broken, exposed by your change. |
| "Refactoring this now will make the fix cleaner" | Separate the fix from the refactor. Mixed commits hide what actually fixed the bug and make rollbacks dangerous. |

## Red Flags

- Making code changes before reproducing the bug
- "Fixed it" with no test proving the original case now passes
- Root cause stated as "human error" — that's a symptom, not a cause
- Fix touches more than the minimum required — likely scope creep or guessing
- No explanation of why the fix works, only what changed
- Original failing test or reproduction case deleted or commented out after fix

## Verification

Before closing the bug:

- [ ] The bug was reproduced before any code was changed
- [ ] A root cause is stated in writing — not just what was changed
- [ ] The fix is surgical: changes only what is required to address the root cause
- [ ] A test exists that failed before the fix and passes after
- [ ] All pre-existing tests still pass
- [ ] The commit message includes the root cause and why the fix is correct
