# A Practical Workflow for Reviewing and Refactoring Code with GitHub Copilot

A reliable refactoring workflow is iterative: diagnose what actually needs improvement, apply focused structural changes, and verify that behavior remains unchanged. GitHub Copilot can assist at each stage across both your IDE and GitHub.com.

## 1. Set a Focused Scope

Select a single function, class, or complete logical unit in your editor—or reference a specific file or pull request on GitHub using `@` in Copilot Chat.

- **Keep it bounded:** A focused selection (such as one 50-line function) yields sharper, safer suggestions than an entire multi-hundred-line file.
- **Verify scope:** Confirm your selection covers the full logical unit without cutting off mid-block.

## 2. Run a Code Review Before Changing Anything

Ask Copilot to critique the code first so you fix real issues rather than refactoring for the sake of change.

> **Prompt:** `"Review this selected code for readability, edge-case bugs, and potential performance bottlenecks (such as unnecessary loops, redundant API calls, or high memory use). List specific findings by severity before suggesting changes."`

- **Verify findings:** Use the output as a concrete checklist of issues (e.g., deeply nested conditionals or an $O(n^2)$ lookup) rather than vague style tweaks.

## 3. Refactor for Clarity and Modularity

Address structure, control flow, and naming before attempting performance optimizations. In your editor, use **Inline Chat** (`Ctrl+I` on Windows/Linux or `Cmd+I` on Mac) to preview changes directly as a diff.

> **Prompt:** `"Refactor this function for clarity. Use guard clauses to flatten nested conditionals, improve unclear variable names, and extract validation logic into a helper where appropriate. Preserve the existing behavior, public interface, and error-handling contracts."`

- **Verify compatibility:** Inspect the proposed diff line-by-line to confirm that callers, function signatures, parameter types, and return values remain intact.

## 4. Evaluate Performance Improvements as Hypotheses

Once the logic is readable, run a targeted pass for runtime or memory efficiency.

> **Prompt:** `"Examine this refactored code for repeated array scans or expensive operations. If a map, set, or caching structure would improve lookup performance, suggest a change and explain the time and memory trade-offs."`

- **Verify trade-offs:** Treat optimization suggestions as hypotheses rather than automatic wins. Confirm they fit your actual data scale, avoid sacrificing clarity for negligible micro-optimizations, and validate critical paths with benchmarks or profiling.

## 5. Verify Behavior and Finalize the Diff

Never merge a refactor without verifying that existing behavior is preserved.

1. **Run or generate tests:** Execute your existing test suite. If coverage is missing, ask Copilot to generate characterization tests alongside your changes:
   > **Prompt:** `"Write unit tests that capture the current behavior of this function, including boundary and null/empty edge cases, so I can verify that the refactor preserves it."`
2. **Review the PR diff:** If you are working in a pull request on GitHub.com and **Copilot code review** is enabled for your plan and repository, assign Copilot as a reviewer to catch any missed issues in the final diff alongside your own inspection.

**Key takeaway:** Use Copilot to surface blind spots and accelerate iteration, but always inspect the diff and verify behavior with tests before merging.
