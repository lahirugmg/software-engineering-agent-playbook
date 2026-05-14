# Skill: interview-me

**Type:** atomic

## Purpose

Extract what a stakeholder actually wants, not what they think they should ask for, through a structured one-question-at-a-time interview. The gap between the stated request and the underlying need is where failed requirements live. This skill closes that gap before any artifact is produced.

## When to Invoke

- The ask is missing who the user is, why they want it, what success looks like, or what the binding constraint is.
- The request is conventional rather than specific ("build me X", "make it faster") and cannot be unpacked without guessing.
- The requester says "interview me", "grill me", or "stress-test my thinking".
- You are tempted to start filling in ambiguous requirements from assumptions.

**Do not invoke** for unambiguous, self-contained requests, or in non-interactive contexts (CI, scheduled runs) — flag underspecification as a blocker instead.

## Workflow

1. **Hypothesize with a confidence number.** Before asking anything, state your current best read of what the stakeholder wants in one sentence, with an honest confidence score (0–100%). If you cannot write the sentence, that is the signal: start here.

2. **Ask one question at a time, with your guess attached.** Format:
   ```
   Q: <one focused question>
   GUESS: <your hypothesis for the answer, with reasoning>
   ```
   Wait for a response before the next question. Batching questions produces surface answers.

3. **Listen for "should want" vs. "actually want".** Watch for answers that pattern-match to best-practice talk ("scalable", "clean", "the standard approach") without specifics. When you hear these, ask: *"If you didn't have to justify this to anyone, what would you actually want?"*

4. **Restate intent when confidence is high.** Use the stakeholder's own words:
   ```
   - Outcome:      <one line>
   - User:         <one line>
   - Why now:      <one line>
   - Success:      <one line>
   - Constraint:   <one line>
   - Out of scope: <one line>
   ```
   **Gate:** The out-of-scope line is non-negotiable. Half of misalignment is silent disagreement about what is not being built.

5. **Confirm — explicit yes only.** "Whatever you think" and "sounds good" are not yes. Re-ask with two concrete options framed as a choice. Loop until you get an explicit yes.
   **Gate:** Do not hand off to requirements-gathering, user-story, or prd-writing until the stakeholder has explicitly confirmed the restate.

## Output

- **Confirmed statement of intent** — the restate from Step 4, with explicit stakeholder confirmation on record.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The ask is clear enough" | If you can't write the desired outcome in one sentence right now, it isn't. Run Step 1 before deciding. |
| "Asking too many questions wastes time" | Building the wrong thing wastes more. A 4–6 question interview is cheap; rework is not. |
| "I'll figure it out as I build" | Discovery during implementation is rework. The switching cost after code exists is 10x what it is now. |
| "They said 'whatever you think', so I'll decide" | "Whatever you think" is delegation, not decision. Re-ask with two concrete options. |

## Red Flags

- Producing requirements, user stories, or a PRD before the stakeholder has confirmed your restate
- Accepting "sounds good" as explicit confirmation
- Asking two or more questions in one message
- Asking a question without your hypothesis attached
- Three or more rounds without your confidence visibly rising — you are asking the wrong questions; step back and reframe
- Omitting the out-of-scope line from the restate

## Verification

- [ ] An explicit hypothesis with confidence number was stated before the first question
- [ ] Questions were asked one at a time, each with a guess attached
- [ ] At least one "what would you actually want?" probe ran when a vague answer was given
- [ ] A concrete restate (Outcome / User / Why now / Success / Constraint / Out of scope) was written
- [ ] The stakeholder confirmed with an explicit yes — not "whatever you think", not silence
