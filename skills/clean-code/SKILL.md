---
name: clean-code
description: Apply Extreme Programming, Clean Code, SOLID, explicit design-pattern decisions, and evidence-based performance checks when implementing, refactoring, or reviewing code.
---

# Clean Code

Use this playbook alongside the target repository's instructions, tooling, and acceptance criteria. Respect the requested scope: review-only work must not edit files. Keep existing behavior unless a requirement explicitly changes it; ask about ambiguities that would affect observable behavior.

## 1. Start with a short XP feedback loop

- Read the acceptance criteria, relevant code and tests, and existing conventions. Identify the smallest change that delivers observable value.
- For a behavior change or bug fix, write or update a focused test first when feasible. Confirm it fails for the intended reason, make it pass with the simplest implementation, then refactor with the test passing. For a risky refactor, capture existing behavior first if it is not covered.
- Test through observable contracts, not private implementation details. Use the smallest relevant feedback loop and existing tools. If a red-green cycle is not feasible, state why and name the verification used instead. Run the relevant tests and project checks after refactoring.

## 2. Make the pattern decision explicit

- Identify the design pressure: for example, behavior that varies, an external dependency, or a complex interface callers must use. Check for an existing pattern or language feature before creating a new abstraction.
- Compare the simplest direct implementation with an applicable pattern. For example, use a Strategy for independently varying rules, an Adapter to isolate a vendor contract, or a Facade to give callers a simpler interface to a subsystem.
- Reuse or apply a pattern when it solves a real problem. Name the pattern, the problem it solves, and the trade-off. If none improves the design, record why the direct approach is better. Pattern assessment is required; ceremony is not.

## 3. Assess maintainability

- Apply Robert C. Martin's Clean Code and SOLID principles where they improve the code: use domain names, keep responsibilities cohesive, prefer small caller-facing contracts, and keep dependencies pointed away from implementation details when it makes a real seam easier to change or test.
- Make control flow, errors, and side effects explicit. Avoid duplicated business rules, speculative abstractions, and refactors unrelated to the requested change. Follow the repository's naming, typing, formatting, and documentation conventions.
- Read the final diff from a maintainer's perspective. Check for unhandled edge cases, leaky abstractions, accidental behavior changes, and tests that would pass even if the implementation were wrong.

## 4. Assess performance

- On every code change, check relevant cost drivers: input growth, allocations and retained memory, repeated I/O or queries, and concurrency or blocking. Call out the affected path and justify the risk level.
- If changing a hot path, making a performance claim, or meeting a performance requirement, capture a baseline and compare it with the change under the same representative workload and environment. Use the repo's profiler or benchmark where available. Report the metric, workload, result, and whether it meets any stated budget.
- For low-risk changes, explain why measurement was unnecessary. If measurement is warranted but blocked, say what was not measured and what would verify it. Never claim an improvement without evidence.

## 5. Close with evidence

Report the behavior checked, the pattern decision, any remaining quality or performance risks, and what verification passed, failed, or could not run. For reviews, lead with actionable findings tied to the relevant code and explain their impact.
