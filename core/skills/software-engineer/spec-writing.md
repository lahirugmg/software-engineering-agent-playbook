# Skill: spec-writing

**Type:** atomic
**Scope:** Produce a clear, testable implementation spec before code is written.

## When to Invoke

- Starting a non-trivial feature or change.
- The requirements are ambiguous and implementation decisions need to be locked in.
- Handing a task to an AI agent — specs are the primary control mechanism over agent output.

## Inputs

- The requirement (ticket, user story, verbal description).
- Relevant constraints: existing APIs, data models, performance targets, security requirements.
- Known unknowns to resolve before implementation begins.

## What a Good Spec Contains

A spec is not documentation. It is a contract that makes the implementation unambiguous and the review verifiable.

### 1. Problem Statement
One paragraph. What problem does this solve, and for whom? Why now?

### 2. Scope
What is explicitly in scope. What is explicitly out of scope. Anything not listed is out of scope.

### 3. Behaviour Specification
The core of the spec. Written as a set of named cases:

```
GIVEN [precondition]
WHEN  [action or input]
THEN  [expected outcome]
```

Cover:
- The happy path
- Boundary conditions (empty, zero, max, min)
- Error cases (invalid input, missing required data, downstream failure)
- Concurrency or ordering concerns if relevant

### 4. Interface Contract
If the change has an API surface (function signature, HTTP endpoint, event schema), define it explicitly:
- Inputs: names, types, required/optional, constraints
- Outputs: structure, types, success and error shapes
- Side effects: what state changes as a result

### 5. Out-of-Scope Concerns
Things that came up during spec writing but are not being addressed now. List them so they don't get silently dropped.

### 6. Open Questions
Decisions not yet made that block implementation. Assign each to a person with a resolution deadline.

## Procedure

1. Draft the scope before writing any behaviour. If you can't bound the problem, you can't spec it.
2. Write the GIVEN/WHEN/THEN cases. Each case should be independently testable.
3. Review the interface contract: would a new engineer be able to implement this without asking you anything?
4. Circulate for sign-off from anyone whose system the change touches.
5. Treat unanswered open questions as blockers. Don't start implementation while they're open.

## Output

A spec document in the above format. Length is proportional to complexity — a simple change needs a simple spec. The test: could a competent engineer implement this from the spec alone, without access to you?

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "This is simple, I don't need a spec" | Simple tasks don't need long specs, but they still need scope and at least one acceptance criterion. A three-line spec is fine. |
| "I'll write the spec after I code it" | That's documentation, not specification. The spec's value is forcing clarity before code — not recording what was built. |
| "Requirements will change anyway" | That's why open questions exist. An incomplete spec is still better than no spec; it names what is unknown rather than hiding it. |
| "The ticket is clear enough" | Tickets describe what to do. Specs define when it's done and what it doesn't do. They serve different purposes. |
| "The stakeholder will just review the code" | Code review is too late to catch scope misunderstandings. The spec review is the right moment. |

## Red Flags

- Starting implementation before scope is bounded in writing
- A spec with no out-of-scope list (silent assumptions are the most dangerous)
- Open questions left unassigned — they will never be resolved
- GIVEN/WHEN/THEN cases that describe only the happy path
- Interface contracts missing error shapes
- Circulating for sign-off from no one — a spec signed off by only its author is not signed off

## Verification

Before handing off to implementation:

- [ ] Problem statement fits in one paragraph
- [ ] Out-of-scope list exists and is non-empty
- [ ] Every behaviour case is GIVEN/WHEN/THEN format and independently testable
- [ ] Interface contract covers inputs, outputs, and error shapes
- [ ] Open questions are assigned to named people with resolution deadlines
- [ ] At least one person other than the author has reviewed and signed off
