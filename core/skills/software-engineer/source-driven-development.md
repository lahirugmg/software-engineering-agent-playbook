# Skill: source-driven-development

**Type:** atomic

## Purpose

Ground every framework-specific implementation decision in official documentation. Training data goes stale, APIs get deprecated, and best practices evolve. This skill ensures that framework-specific code is verifiable by the reader — every pattern traces back to an authoritative source they can check.

## When to Invoke

- Building boilerplate, starter code, or patterns that will be copied across a project.
- Implementing features where the framework's recommended approach matters (forms, routing, data fetching, state management, auth).
- Reviewing or improving code that uses framework-specific patterns.
- Any time you are about to write framework-specific code from memory.

**Do not invoke** for changes where correctness does not depend on a specific version (renames, typos, moving files), or for pure logic that works the same across versions.

## Workflow

1. **Detect stack and versions.** Read the project's dependency file before writing any framework-specific code. State what you found explicitly:
   ```
   STACK DETECTED:
   - React 19.1.0 (from package.json)
   - Next.js 15.0.0
   → Fetching docs for the relevant patterns.
   ```
   If versions are missing or ambiguous, ask the user. Do not guess — the version determines which patterns are correct.

2. **Fetch the relevant documentation page.** Not the homepage, not a search — the specific page for the feature being implemented. Source hierarchy in order of authority:
   1. Official documentation (react.dev, docs.djangoproject.com, etc.)
   2. Official blog / changelog
   3. Web standards references (MDN, web.dev)
   4. Browser/runtime compatibility (caniuse.com)

   Not authoritative — never cite as primary sources: Stack Overflow, blog posts (even popular ones), AI-generated summaries, or your own training data.

3. **Implement following documented patterns.** Use the API signatures from the docs, not from memory. If the docs show a new way to do something, use the new way. If the docs deprecate a pattern, do not use the deprecated version. If the docs do not cover something, flag it as unverified.
   **Gate:** When official docs conflict with existing project code, surface the conflict explicitly and ask for direction — do not silently pick one.

4. **Cite your sources.** Every framework-specific pattern gets a citation. In conversation, state which version's documentation was read and link to the specific page. In code, a short comment with the URL is appropriate for non-obvious decisions. If a pattern could not be verified, say so explicitly:
   ```
   UNVERIFIED: Could not find official documentation for this pattern.
   This is based on training data and may be outdated. Verify before use.
   ```

## Output

- Framework-specific code that follows current documented patterns.
- Source citations for every framework-specific decision.
- Explicit flags for any patterns that could not be verified.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'm confident about this API" | Confidence is not evidence. Training data contains outdated patterns that look correct but break against current versions. |
| "Fetching docs wastes tokens" | Hallucinating an API wastes more. The user debugs for an hour, then discovers the function signature changed. One fetch prevents hours of rework. |
| "I'll just mention it might be outdated" | A disclaimer does not help. Either verify and cite, or clearly flag it as unverified. Hedging is the worst option. |
| "This is a simple task, no need to check" | Simple tasks with wrong patterns become templates. The user copies a deprecated pattern into ten components before discovering the modern approach. |

## Red Flags

- Writing framework-specific code without checking the version-specific docs
- Using "I believe" or "I think" about an API instead of citing the source
- Citing Stack Overflow or blog posts as the primary authority
- Using a deprecated API because it appears in training data
- Not reading the dependency file before implementing
- Delivering framework-specific code without source citations for non-obvious decisions

## Verification

- [ ] Framework and library versions were identified from the dependency file before implementing
- [ ] Official documentation was fetched for each framework-specific pattern used
- [ ] Code follows the patterns shown in the current version's documentation
- [ ] Non-trivial decisions include source citations with full URLs
- [ ] No deprecated APIs are used (checked against migration guides)
- [ ] Conflicts between docs and existing code were surfaced explicitly, not resolved silently
- [ ] Anything that could not be verified is explicitly flagged as unverified
