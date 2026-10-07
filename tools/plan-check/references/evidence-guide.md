# Evidence Guide: Where to find evidence and what good looks like

This guide specifies where key evidence lives in an evaluation package or draft submission and establishes the criteria distinguishing acceptable evidence from failing evidence.

---

## 1. Diagnosis and Root Cause Evidence

### Where it lives
- In practice submissions / packages: under the `## Diagnosis` or `## Root Cause` section of the plan block, evaluated against the error trace and log snippets in `## Repro Evidence` or the issue description.
- In student drafts: `plan.md` under `## Diagnosis` compared against the repro commands and outputs quoted from Unit 2.

### What good looks like
- Pinpoints the exact mechanism causing the failure (e.g., specific missing nil-check, off-by-one boundary, unhandled edge-case input, or incorrect type coercion).
- Aligns directly with the stack trace, error message, or incorrect output demonstrated in the reproduction.
- Distinguishes the fundamental cause from secondary symptoms.

### What bad looks like
- Symptom patching (e.g., silencing an error or wrapping everything in a generic `try/except` without fixing the underlying state).
- Contradicts the captured reproduction logs or claims a failure mechanism not present in the repro.
- Vague hand-waving (e.g., "the function has bugs when processing data").

---

## 2. Scope and Boundary Evidence

### Where it lives
- In packages / drafts: under `## Scope`, `## Non-Goals`, and `## Files to touch` in the plan, read against `repo-facts` and the original issue prompt.

### What good looks like
- Strictly limits file modifications to the component containing the bug.
- Clearly states non-goals (what will intentionally not be changed or refactored).
- Keeps the diff minimal, targeting only what is required to pass the test plan.

### What bad looks like
- Unsolicited refactoring, renaming variables across files, or reformatting unrelated code.
- "Drive-by" feature additions or speculative abstractions not requested in the issue.
- Touching files unrelated to the root cause codepath.

---

## 3. Implementation Approach and Clarity

### Where it lives
- In packages / drafts: under `## Approach` or `## Implementation Steps` in `plan.md`.

### What good looks like
- Precise, unambiguous pseudocode or plain-language logic changes.
- Names the specific functions, methods, or modules being updated.
- An unfamiliar engineer could implement the fix directly without making ad-hoc design decisions.

### What bad looks like
- Generic advice (e.g., "update logic to be faster" or "handle errors properly").
- Incomplete plans that stop halfway through the execution path.
- Ambiguous conditional statements that leave boundary behavior undefined.

---

## 4. Test Plan and Validation Evidence

### Where it lives
- In packages / drafts: under `## Test Plan` or `## Validation` in `plan.md`, read against the repro commands.

### What good looks like
- Re-runs the exact reproduction steps or automated test suite command.
- Explicitly states the expected output showing the defect is resolved (e.g., "exits 0", specific output string, or test suite passes).
- Includes validation for edge cases or related non-broken behavior to ensure no regression.

### What bad looks like
- "Run tests and make sure they pass" without specifying the command or target suite.
- Re-running a command without defining what output constitutes a pass versus a fail.
- Relying on manual inspection when an automated reproduction command is available.

---

## 5. Risks, Unknowns, and Edge Cases

### Where it lives
- In packages / drafts: under `## Risks and Unknowns` in `plan.md`.

### What good looks like
- Identifies realistic potential regressions (e.g., backwards compatibility, performance impact on large inputs, concurrency concerns).
- Explicitly states what is still unknown prior to implementation and how it will be validated during build.

### What bad looks like
- "No risks" or omitting the section entirely on non-trivial logic changes.
- Claiming complete certainty about unverified dependencies or external behaviors.

---

## 6. Thread and Repository Conventions

### Where it lives
- In packages / drafts: `comment.md` read against `thread-highlights`, maintainer feedback, and repository conventions.

### What good looks like
- Directly answers questions or follows constraints posed by maintainers in the thread.
- Adheres to repository communication norms (concise, technical, courteous, no boilerplate spam).
- Accurately summarizes the diagnosis and proposed approach without omitting key architectural impacts.

### What bad looks like
- Ignoring explicit maintainer requests (e.g., maintainer asked not to touch an API, but the plan modifies it).
- Empty, generic comments (e.g., "I have a plan, will submit PR soon").
- Disagreement between `plan.md` and the summary posted in `comment.md`.