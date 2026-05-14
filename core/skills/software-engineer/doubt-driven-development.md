# Skill: doubt-driven-development

**Type:** atomic

## Purpose

Subject every non-trivial decision to a fresh-context adversarial review before it stands. Long sessions accumulate context that quietly turns assumptions into facts. This skill materialises a reviewer biased to disprove — not approve — while course-correction is still cheap. It is distinct from code-review: code-review is a verdict on a finished artifact; this is an in-flight posture applied to decisions as they are made.

## When to Invoke

A decision is non-trivial when at least one of the following is true:
- It introduces or modifies branching logic.
- It crosses a module or service boundary.
- Its correctness depends on context the type system cannot verify (thread safety, ordering, idempotence, invariants).
- Its blast radius is hard to reverse (production deploy, data migration, public API change).

Apply when:
- About to commit non-trivial code or make an architectural decision under uncertainty.
- About to claim a non-obvious property ("this is safe", "this scales", "this matches the spec").
- Working in code you do not fully understand.

**Do not invoke** for mechanical operations (renames, formatting, file moves), one-line changes with obvious correctness, or when the user has explicitly asked for speed over verification.

## Workflow

This skill runs as a bounded loop — at most 3 cycles, never more.

1. **CLAIM — surface what stands.** Name the decision in two or three lines:
   ```
   CLAIM: <the assertion being made>
   WHY THIS MATTERS: <what fails if this claim is wrong>
   ```
   If you cannot write the claim compactly, you have a vibe, not a decision. Surface it before scrutinising it.

2. **EXTRACT — smallest reviewable unit.** Isolate the artifact and the contract. Strip your reasoning — if you hand over conclusions, you get back validation of conclusions. The unit must be small enough to hold in mind in one read. If the artifact is a 500-line PR, decompose it first.
   - Code: the diff or the function — not the whole file.
   - Decision: the proposal in 3–5 sentences plus the constraints it must satisfy.

3. **DOUBT — invoke a fresh-context reviewer.** The reviewer's prompt must be adversarial:
   ```
   Adversarial review. Find what is wrong with this artifact.
   Assume the author is overconfident. Look for:
   - Unstated assumptions
   - Edge cases not handled
   - Hidden coupling or shared state
   - Ways the contract could be violated
   - Failure modes under unexpected input

   Do NOT validate. Do NOT summarise. Find issues, or state
   explicitly that you cannot find any after thorough examination.

   ARTIFACT: <artifact>
   CONTRACT: <contract>
   ```
   Pass ARTIFACT + CONTRACT only. Do not pass the CLAIM — it biases the reviewer toward agreement.
   **Gate:** The reviewer's prompt must be adversarial ("find issues"), never validating ("is this good?").

4. **RECONCILE — classify findings.** Re-read the artifact text against each finding before classifying. Do not rubber-stamp the reviewer — it lacks your context. Classify each finding in precedence order:
   1. **Contract misread** — fix the contract, reclassify.
   2. **Valid + actionable** — change the artifact, re-loop.
   3. **Valid trade-off** — document the trade-off explicitly.
   4. **Noise** — note it and ask whether better contract wording would have prevented the false flag.

5. **STOP — bounded loop.** Stop when: next iteration returns only trivial or already-considered findings; or 3 cycles completed (escalate to user, do not grind a fourth alone); or the user says "ship it".
   **Gate:** If after 3 cycles the reviewer still surfaces substantive issues, surface this to the user — three unresolved cycles is information about the artifact, not a reason to keep looping.

## Output

- A classification of all findings (actionable / trade-off / noise) with rationale.
- Updated artifact if any findings were actionable.
- Explicit documentation of accepted trade-offs.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'm confident, skip the doubt step" | Confidence correlates poorly with correctness on novel problems. Moments of certainty are when blind spots hide. |
| "I'll do doubt at the end with code-review" | code-review is a final gate. Doubt-driven catches wrong directions early when course-correction is cheap. By PR time it is too late. |
| "If I doubt every step I'll never ship" | The skill applies to non-trivial decisions, not every keystroke. Re-read "When to Invoke." |
| "The reviewer disagreed so I was wrong" | The reviewer lacks your context. Disagreement is information, not verdict. Re-read the artifact, classify, then decide. |

## Red Flags

- Invoking for a one-line rename or formatting change
- Treating reviewer output as authoritative without re-reading the artifact text
- Looping more than 3 cycles without escalating to the user
- Prompting the reviewer with "is this good?" instead of "find issues"
- Passing the CLAIM to the reviewer
- Doubting only after committing — that is code-review, not doubt-driven development

## Verification

After applying this skill to a non-trivial decision:

- [ ] The decision was named explicitly as a CLAIM before standing
- [ ] The reviewer received ARTIFACT + CONTRACT only — not the CLAIM, not your reasoning
- [ ] The reviewer's prompt was adversarial ("find issues"), not validating
- [ ] Findings were classified against the artifact text using the precedence order
- [ ] A stop condition was met (trivial findings, 3 cycles, or user override)
- [ ] Accepted trade-offs are documented explicitly
