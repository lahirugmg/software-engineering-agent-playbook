# Skill: idea-refine

**Type:** atomic

## Purpose

Turn a vague idea into a sharp, scoped proposal worth handing to engineering. Runs divergent thinking first (expand options, stress-test assumptions) and convergent thinking second (cluster, evaluate, commit). The output is a concrete one-pager — not a recommendation to "think more".

## When to Invoke

- An idea exists but is too vague to specify or estimate.
- You need to stress-test assumptions before committing to a direction.
- Multiple directions are possible and trade-offs need to be made explicit before choosing one.
- A confirmed intent from interview-me needs to be developed into a concrete proposal.

**Do not invoke** when the direction is already decided and the task is to execute — that is spec-writing, not idea refinement.

## Workflow

1. **Restate and sharpen (divergent).** Reframe the idea as a "How Might We" problem statement. Then ask 3–5 sharpening questions — no more — focused on: who this is for, what success looks like, what the real constraints are, what has been tried, and why now.
   **Gate:** Do not proceed to generating variations until you can state who benefits and what success looks like.

2. **Generate variations.** Produce 5–8 meaningfully different directions using lenses: inversion ("what if we did the opposite?"), constraint removal, audience shift, combination with an adjacent idea, 10x simplification, and 10x scale. Push beyond the first idea. Quality over quantity — 5 considered variations beat 20 shallow ones.

3. **Evaluate and converge.** Cluster variations that resonated into 2–3 distinct directions. Stress-test each against: user value (painkiller vs. vitamin?), feasibility (what is the hardest part?), and differentiation (would someone switch?). Surface hidden assumptions explicitly — what are you betting is true, what could kill this, what are you choosing to ignore.
   **Gate:** Be honest, not supportive. If a direction is weak, say so with specificity. A yes-machine is not an ideation partner.

4. **Produce the one-pager.** Write a concrete artifact:
   ```
   # [Idea Name]
   ## Problem Statement
   ## Recommended Direction
   ## Key Assumptions to Validate (with how to test each)
   ## MVP Scope (what's in, what's explicitly out)
   ## Not Doing (and Why)
   ## Open Questions
   ```
   **Gate:** The "Not Doing" list is the most valuable section. Focus is saying no to good ideas. Make trade-offs explicit before handing off.

5. **Confirm before handing off.** Ask the stakeholder to confirm the recommended direction before any spec, estimate, or implementation work begins.

## Output

- **Idea one-pager** — problem statement, recommended direction, assumptions to validate, MVP scope, not-doing list, open questions.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The idea is clear, we can go straight to spec" | A clear idea is not a validated idea. The one-pager forces trade-off clarity that spec-writing doesn't. |
| "More options means more confusion" | Controlled divergence before convergence produces better decisions than committing to the first idea. |
| "We don't have time to stress-test" | Untested assumptions are the primary killer of good ideas. Surfacing them now is cheap; discovering them in engineering is not. |
| "The Not Doing list is obvious" | If it's obvious, it's fast to write. If it's not obvious, it's critical to write. Either way, it belongs in the output. |

## Red Flags

- Generating variations without first understanding who this is for
- Skipping assumption surfacing and going straight to a recommendation
- A recommendation without a "Not Doing" list
- Phase 3 (one-pager) produced without running Phases 1 and 2
- Validating weak ideas instead of pushing back with specificity

## Verification

- [ ] A "How Might We" problem statement exists
- [ ] Target user and success criteria are defined before variations were generated
- [ ] Multiple meaningfully different directions were explored (not just variations on the first idea)
- [ ] Hidden assumptions are explicitly listed with validation strategies for each
- [ ] A "Not Doing" list makes trade-offs explicit
- [ ] The stakeholder confirmed the recommended direction before any downstream work began
