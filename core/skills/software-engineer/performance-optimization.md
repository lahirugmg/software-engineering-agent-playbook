# Skill: performance-optimization

**Type:** atomic

## Purpose

Identify and fix performance bottlenecks through measurement, not intuition. Performance work without measurement is guessing — and guessing adds complexity without improving what matters. Profile first, find the actual bottleneck, fix it, measure again.

## When to Invoke

- Performance requirements exist in the spec (load time budgets, response time SLAs).
- Users or monitoring report slow behaviour.
- Core Web Vitals scores are below acceptable thresholds.
- A change is suspected to have introduced a regression.
- Building features that will handle large datasets or high traffic.

**Do not invoke** before you have evidence of a problem. Premature optimisation adds complexity that costs more than the performance it gains.

## Workflow

```
MEASURE → IDENTIFY → FIX → VERIFY → GUARD
```

1. **Establish a baseline.** Measure before any change. Without a baseline, you cannot confirm whether a fix improved anything. Use synthetic measurement (Lighthouse, DevTools Performance tab) for reproducibility and real-user measurement (web-vitals library, APM) to confirm improvements reach actual users. State the baseline explicitly before touching code.
   **Gate:** Do not proceed to identifying bottlenecks without a numeric baseline. "It feels slow" is not a baseline.

2. **Identify the actual bottleneck — do not assume.** Use profiling to find where time is actually spent. Common starting points by symptom:
   - Slow first page load → bundle size, code splitting, render-blocking resources.
   - Slow interactions → main thread blocking, synchronous operations, re-render cascades.
   - Slow API responses → slow queries (check EXPLAIN), N+1 queries, missing indexes.
   - Memory growth → leaking event listeners, accumulating cache, closures holding references.
   The bottleneck is often not where intuition points.

3. **Fix the specific bottleneck.** Change only what measurement identified. Do not optimise adjacent code "while you're there." Common fixes by category:
   - **Bundle size:** code splitting, lazy loading, tree shaking, removing unused dependencies.
   - **Render performance:** memoisation (only after profiling shows it helps), virtualisation for long lists, deferring off-screen work.
   - **Database:** query-specific indexes, select only the columns needed, batch queries to eliminate N+1.
   - **Caching:** cache expensive computations or I/O-bound results — only where TTL is safe.

4. **Verify the improvement.** Measure after the fix using the same method as the baseline. Confirm the number improved. Confirm no regression was introduced in adjacent metrics (fixing LCP should not worsen CLS). A fix that improves the profiled case but regresses another is a net loss.
   **Gate:** Do not declare the optimisation complete without a before/after number comparison.

5. **Guard against regression.** Add a monitoring check, a performance test, or a budget assertion so this bottleneck does not silently return. A fix without a guard is a temporary fix.

## Core Web Vitals Reference Targets

| Metric | Good | Needs Improvement | Poor |
|---|---|---|---|
| LCP (Largest Contentful Paint) | ≤ 2.5s | ≤ 4.0s | > 4.0s |
| INP (Interaction to Next Paint) | ≤ 200ms | ≤ 500ms | > 500ms |
| CLS (Cumulative Layout Shift) | ≤ 0.1 | ≤ 0.25 | > 0.25 |

## Output

- Numeric baseline and post-fix measurement.
- Description of the bottleneck found and the fix applied.
- Confirmation that no regression was introduced in adjacent metrics.
- A monitoring check, performance test, or budget assertion guarding the fix.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I know where the bottleneck is" | Profile first. Developer intuition about performance bottlenecks is wrong more often than it is right. |
| "Memoising this is obviously right" | Memoisation has a cost (memory, cache invalidation complexity). Profile first — if the function is not on the hot path, memoisation makes things worse, not better. |
| "We'll add performance tests later" | Without a guard, the fix regresses silently. Later rarely happens. Add the guard now. |
| "The dev build is slow but prod will be fine" | Measure in production-like conditions. Dev builds have different bundle characteristics, no minification, and extra instrumentation. |

## Red Flags

- Optimising without a numeric baseline
- Fixing a bottleneck that profiling did not identify
- Adding memoisation or caching without measuring that it helps
- Declaring the optimisation complete without a before/after measurement
- Optimising code adjacent to the identified bottleneck "while here"
- No guard added after the fix

## Verification

Before closing the task:

- [ ] Numeric baseline established before any code was changed
- [ ] Bottleneck identified by profiling, not intuition
- [ ] Fix targets only the identified bottleneck
- [ ] Post-fix measurement shows improvement against the baseline
- [ ] No adjacent metric regressed
- [ ] A monitoring check, performance test, or budget assertion guards against recurrence
