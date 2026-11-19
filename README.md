# Agent Skills

A personal library of reusable engineering playbooks. Keep project-specific rules, test commands, and acceptance criteria in the projects they belong to.

## Skills

| Skill | When to use it |
| --- | --- |
| [Clean Code](skills/clean-code/SKILL.md) | Implementing, refactoring, or reviewing code with an XP feedback loop, an explicit design-pattern decision, and maintainability and performance checks. |
| [Test Architecture](skills/test-architecture/SKILL.md) | Starting or reviewing a web test suite using current, version-compatible framework guidance, including when and how to add network guards. |

## Use in another project

1. **On demand:** In the target project's chat, share the chosen `SKILL.md` as a GitHub link or accessible local path. Ask the agent to read it and apply it against the target repository's instructions and acceptance criteria. The session needs access to the file.
2. **As a Copilot slash command:** Copy the chosen skill folder into your personal `~\.copilot\skills\` directory, or into the target project's `.github\skills\` if the team should share it. Start a fresh chat and type `/clean-code` or `/test-architecture`. Keep one copy per intended scope to avoid duplicates.

First try it on a small change or request a read-only review:

```text
/clean-code Review my current diff against the acceptance criteria. Assess the
design-pattern decision, maintainability, test coverage, and performance risk.
Report findings without editing files.
```

For a new test suite, start with a recommendation before scaffolding:

```text
/test-architecture Propose a starter structure for Playwright Test.
Check the current official guidance and release notes. Do not edit files yet.
```

Skills guide the agent; they do not enforce policy in CI. Use the project's tests, linters, and performance checks for automated enforcement.

## Related upstream skills

[Matt Pocock's Skills for Real Engineers](https://github.com/mattpocock/skills) provides `diagnosing-bugs`, `code-review`, and `improve-codebase-architecture`. Use those from upstream when you need a specialized investigation or review.
