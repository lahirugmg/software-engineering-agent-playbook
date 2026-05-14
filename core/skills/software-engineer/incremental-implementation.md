# Skill: incremental-implementation

**Type:** atomic

## Purpose

Build changes in thin vertical slices — implement one piece, test it, verify it, commit it, then expand. Every increment leaves the system in a working, testable state. Prevents bugs from compounding across a large unverified change set and makes rollback possible at any point.

## When to Invoke

- Implementing any change that touches more than one file.
- Building a new feature from a task breakdown.
- Refactoring existing code.
- Any point where you are tempted to write more than ~100 lines before running tests.

**Do not invoke** for single-file, single-function changes where the scope is already minimal.

## Workflow

The increment cycle: **Implement → Test → Verify → Commit → Next slice.**

1. **Choose a slicing strategy.**
   - **Vertical slices (preferred):** Each slice delivers working end-to-end functionality (e.g., create → list → edit → delete as separate slices, each with DB + API + UI).
   - **Contract-first:** Define the API contract first, then implement backend and frontend in parallel against it, then integrate.
   - **Risk-first:** Tackle the riskiest or most uncertain piece first. If it fails, you discover it before investing in the rest.

2. **Simplicity first — before writing each slice.** Ask: what is the simplest thing that could work? Implement the naive, obviously-correct version. Optimize only after correctness is proven with tests. Three similar lines of code is better than a premature abstraction.
   **Gate:** No abstractions without at least three concrete use cases already existing. No features beyond the current slice's scope.

3. **Scope discipline per slice.** Touch only what the current slice requires. If you notice something worth improving outside the slice, note it and continue — do not fix it inline. Mixed changes make rollback and review harder.

4. **Test and verify after each slice.** Run the test suite. Confirm the build succeeds. If you introduced new behaviour, confirm it works as expected. Do not move to the next slice with a broken build or failing tests.
   **Gate:** The project must build and existing tests must pass after every single slice.

5. **Commit each slice.** A descriptive commit per slice. If the feature isn't ready for users, wrap it in a feature flag — do not leave uncommitted changes accumulating between slices.

## Output

- A sequence of committed, individually-verified slices.
- Each slice: builds cleanly, passes tests, commits with a descriptive message.
- At the end: the full feature works end-to-end as specified, with no uncommitted changes.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll test it all at the end" | Bugs compound. A bug in Slice 1 corrupts Slices 2–5. Test each slice. |
| "It's faster to do it all at once" | It feels faster until something breaks and you can't find which of 500 changed lines caused it. |
| "This refactor is small enough to include" | Refactors mixed with features make both harder to review and debug. Separate them. |
| "These changes are too small to commit separately" | Small commits are free. Large commits hide bugs and make rollbacks painful. |
| "Let me just quickly add this too" | Scope expansion during execution is how slices turn into monoliths. Note it, don't do it. |

## Red Flags

- More than 100 lines written without running tests
- Multiple unrelated concerns in a single slice
- Build or tests broken between slices
- Large uncommitted changes accumulating
- Abstractions introduced before three concrete use cases exist
- Files outside the task scope modified "while I'm here"

## Verification

After completing all slices for a task:

- [ ] Each slice was individually tested and committed
- [ ] The full test suite passes
- [ ] The build is clean
- [ ] The feature works end-to-end as specified
- [ ] No uncommitted changes remain
- [ ] No abstractions were introduced without at least three concrete uses
