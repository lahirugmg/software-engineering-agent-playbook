# Skill: agent-output-review

**Type:** atomic
**Scope:** Verification of AI-generated code before it is merged or deployed. Distinct from code-review — this targets the failure modes specific to AI output.

## When to Invoke

- Code was generated or substantially modified by an AI agent.
- Before committing or merging AI-produced changes.
- When the implementation looks plausible but hasn't been deeply read.

## Inputs

- The AI-generated code (diff or files).
- The original task or prompt given to the agent.
- Any context the agent had access to (relevant docs, existing code).

## AI-Specific Failure Modes to Check

These failure modes are rare in human-authored code but common in AI output:

| Failure mode | What to look for |
|---|---|
| **Hallucinated APIs** | Function calls, library methods, or config keys that don't exist |
| **Confident wrongness** | Code that looks correct but produces wrong results at boundaries |
| **Scope creep** | Changes beyond what the task required — extra abstractions, new files, modified unrelated code |
| **Copy-paste drift** | Repeated patterns where one instance was updated but others weren't |
| **Missing edge cases** | Happy path only — no handling for empty input, nil, zero, concurrent access |
| **Test theatre** | Tests that always pass regardless of implementation |
| **Security regressions** | Input validation removed, auth checks weakened, secrets introduced |
| **Unused scaffolding** | Generated imports, variables, or functions that are never referenced |

## Procedure

1. **Re-read the original task.** Confirm you know what was asked before reading the output.

2. **Verify API and library calls exist.** For any unfamiliar method call, confirm it exists in the version in use. Don't assume.

3. **Trace the logic, don't skim.** AI code often looks correct on first read. Slow down on conditionals, loops, and error paths.

4. **Check every edge case against the code, not the description.** The agent may have described edge case handling without implementing it.

5. **Run the tests.** If tests are included, confirm they can actually fail: delete the core logic and verify at least one test breaks.

6. **Check scope.** List every file changed. Flag anything outside the task boundary.

7. **Security pass.** Apply the security checklist from @core/knowledge/security-guidelines.md.

## Output

- **Go / No-go** decision with brief rationale.
- Blocking issues listed with file:line references.
- Confirmed: what the agent got right (calibrates trust for future agent use).
- Risk rating: Low / Medium / High — based on scope of changes and confidence in correctness.
