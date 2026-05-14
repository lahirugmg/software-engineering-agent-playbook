# Skill: code-simplification

**Type:** atomic

## Purpose

Reduce complexity in working code while preserving exact behaviour. The goal is not fewer lines — it is code that a new team member understands faster. Every simplification must pass one test: "Would a new team member understand this faster than the original?"

## When to Invoke

- After a feature is working and tests pass, but the implementation feels heavier than it needs to be.
- During code review when readability or complexity is flagged.
- When encountering deeply nested logic, long functions (50+ lines), or unclear names.
- When refactoring code written under time pressure.

**Do not invoke** when you do not yet fully understand what the code does (comprehend before you simplify), when the code is about to be rewritten entirely, or when the code is performance-critical and the simpler version would be measurably slower.

## Workflow

1. **Understand before touching (Chesterton's Fence).** Before changing or removing anything, answer: What is this code's responsibility? What calls it? What are the edge cases? Why might it have been written this way? Check git blame for the original context.
   **Gate:** If you cannot answer these questions, you are not ready to simplify. Read more context first.

2. **Identify simplification opportunities.** Scan for concrete signals:
   - Deep nesting (3+ levels) → extract to guard clauses or helper functions.
   - Long functions (50+ lines) → split into focused functions with descriptive names.
   - Nested ternaries → replace with if/else chains or lookup objects.
   - Generic names (`data`, `result`, `temp`) → rename to describe the content.
   - Duplicated logic (same 5+ lines in multiple places) → extract to a shared function.
   - Dead code (unreachable branches, unused variables) → remove after confirming it is truly dead.
   - Unnecessary abstractions (wrapper that adds no value) → inline.
   - Comments explaining *what* the code does → delete them; the code already says this.
   - Comments explaining *why* the code does it → keep them; they carry intent the code cannot express.

3. **Apply changes incrementally.** Make one simplification at a time. Run tests after each change. Do not batch multiple simplifications into a single untested change — if something breaks, you need to know which one caused it.
   **Gate:** Submit simplification changes separately from feature or bug fix changes. A PR that refactors and adds a feature is two PRs.

4. **Verify the result.** Compare before and after: is the simplified version genuinely easier to understand? Did you introduce patterns inconsistent with the codebase? Would a teammate approve this? If the "simplified" version is harder to understand or review, revert.

## Output

- Simplified code with all existing tests passing and the build clean.
- A clean, reviewable diff — no unrelated changes mixed in.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "It's working, no need to touch it" | Working code that is hard to read will be hard to fix when it breaks. Simplifying now saves time on every future change. |
| "Fewer lines is always simpler" | A 1-line nested ternary is not simpler than a 5-line if/else. Simplicity is comprehension speed, not line count. |
| "I'll simplify this unrelated code while I'm here" | Unscoped simplification creates noisy diffs and risks regressions in code you did not intend to change. Stay focused. |
| "This abstraction might be useful later" | Do not preserve speculative abstractions. If it is not used now, it is complexity without value. |
| "I'll refactor while adding this feature" | Separate refactoring from feature work. Mixed changes are harder to review, revert, and understand in history. |

## Red Flags

- Simplification that requires modifying tests to pass (you changed behaviour, not just form)
- "Simplified" code that is longer and harder to follow than the original
- Renaming things to match your preferences rather than project conventions
- Removing error handling because "it makes the code cleaner"
- Simplifying code you do not fully understand
- Batching many simplifications into one large, hard-to-review commit
- Changes outside the scope of the current task without being asked

## Verification

After a simplification pass:

- [ ] All existing tests pass without modification
- [ ] Build succeeds with no new warnings
- [ ] Linter/formatter passes
- [ ] Each simplification is a separate, reviewable, incremental change
- [ ] Simplified code follows project conventions
- [ ] No error handling was removed or weakened
- [ ] No dead code was left behind
- [ ] The diff contains no unrelated changes
