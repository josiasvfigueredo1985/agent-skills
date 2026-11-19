---
name: test-architecture
description: Design, review, or scaffold a web test automation architecture after researching current guidance for the project's framework and runner. Start with Playwright, Selenium, or WebdriverIO when no stack has been chosen.
---

# Test Architecture

Use the chosen framework and runner as the guide for structure and lifecycle. Do not copy one directory layout or abstraction stack across frameworks. Preserve the project's acceptance criteria and behavior. If asked only for a recommendation or review, do not change files.

## 1. Identify the stack and constraints

- For an existing suite, read manifests, lockfiles, runner configuration, sample tests, and CI. Identify the installed framework and runner versions, language, supported browsers, and how tests run locally and in CI.
- For a new suite with no stack selected, compare Playwright, Selenium, and WebdriverIO against the actual needs: team language and experience, browser and execution targets, existing infrastructure, and maintenance cost. Consider other tools when a requirement warrants it. Recommend a choice with trade-offs and confirm it before scaffolding if the choice is still open.

## 2. Research current guidance on every invocation

- Search the web before recommending a structure, even if you know the framework. Check official best practices, recent release notes or migration guides, and current ecosystem developments for the selected framework and runner. Look for changes that affect structure, such as new runner capabilities, deprecated extensions, or changes to supported integrations. When comparing frameworks, do this for each candidate. Use recent maintainer or community examples when official guidance does not settle a design choice; distinguish them from official recommendations.
- Search using public framework and runner names, versions, and generic design questions. Do not send private source code, configuration, or test data to external tools.
- Check both the current stable release and the version the project actually uses. Use version-compatible guidance for existing projects. For new projects, prefer the latest compatible stable release unless a constraint justifies another choice. Do not upgrade or migrate an existing project merely because a newer release exists.
- Report the research date, versions checked, links, and which architectural decisions the sources changed or confirmed. If web access or version verification is unavailable, state what could not be verified and label recommendations provisional. Never claim they reflect current guidance without checking.

### Starting points, not templates

| Stack | Official starting points | What to verify |
| --- | --- | --- |
| Playwright | [Best practices](https://playwright.dev/docs/best-practices), [release notes](https://playwright.dev/docs/release-notes) | When using Playwright Test, consider native fixtures, projects, browser-context isolation, and traces before adding wrappers. |
| Selenium | [Test practices](https://www.selenium.dev/documentation/test_practices/), [releases](https://github.com/SeleniumHQ/selenium/releases) | Selenium WebDriver is browser automation, not the test runner. Verify guidance for the language binding and chosen runner; define driver creation and teardown with that runner, and use Grid only when remote execution or scale warrants it. |
| WebdriverIO | [Getting started](https://webdriver.io/docs/gettingstarted/), [releases](https://github.com/webdriverio/webdriverio/releases) | Distinguish standalone mode from the WDIO test runner. With the runner, check its config, framework adapter, hooks, and capabilities before adding your own lifecycle or orchestration. |

## 3. Propose the smallest useful structure

- Show a concise directory tree, explain the responsibility of each proposed part, and show how one representative test flows through the runner, setup, user actions, assertions, teardown, and reporting.
- Prefer runner-native configuration, lifecycle, waits, and reporting. Keep scenarios readable and business assertions visible. Add page or component objects when they hide meaningful UI complexity or reuse; record why each extra layer earns its place.
- Define how tests isolate browser state and data, including cleanup and parallel execution where needed. Choose stable locators and observable readiness conditions over fixed sleeps. Ensure failures retain useful evidence without exposing sensitive data.
- Assess maintainability and feedback speed. Identify likely sources of flakiness or slow runs, such as repeated setup, shared data collisions, or expensive browser matrices. Separate what the suite needs now from what can wait.

### Evaluate network guards

For browser suites, explicitly decide whether to add network guards. They can catch backend failures that never surface in the UI, but they supplement rather than replace assertions about what the user sees. Record the decision even when guards are not needed.

- **Scope monitoring explicitly.** Use a shared matcher for both guards. Monitor only configured application REST origins and paths and explicitly enabled GraphQL endpoints. Leave unrelated traffic, such as analytics and other third-party requests, out of scope. Keep the list of monitored endpoints separate from the list of expected errors.
- **Keep synchronization separate from failure detection.** Where synchronization is needed, prefer an action-specific wait for an identified request with a configurable time limit. Do not wait for global network idle or add sleeps after every action. A best-effort wait timing out does not disable error monitoring; a response required by the scenario needs its own explicit check. Network completion is not proof of rendering or hydration; use runner-native assertions or an application readiness signal for the UI.
- **Fail on unexpected monitored errors.** Detect HTTP 4xx/5xx, request failures, and GraphQL responses with a non-empty `errors` array even on HTTP 200. Surface failures at the next guard checkpoint or during teardown if they arrive after a wait ends. Include useful request and error metadata without exposing secrets or sensitive payloads.
- **Keep negative scenarios deterministic.** Immediately before the action, declare a one-shot expectation for the next matching monitored request, such as `POST /api/orders` returning `409` or the `CreateOrder` GraphQL operation returning an `errors` array. The expectation must be fulfilled exactly once: fail if the request is missing, unfinished, unexpectedly successful, or returns a different error. Continue monitoring other traffic and assert the intended UI outcome separately. Use persistent exceptions only for narrowly scoped, documented known issues with a reason and a removal condition.
- **Use the runner's lifecycle.** Register and dispose of observers per test using the framework's fixtures or hooks. Verify event and response-body support for the installed browser, binding, and runner before promising equivalent coverage across Playwright, Selenium, and WebdriverIO. Document gaps rather than treating missing telemetry as a pass.

## 4. Validate before expanding

If implementation was requested, start with one representative test. Verify it runs alone and through the intended runner configuration, including parallel isolation if the design depends on it. Check that failures produce useful diagnostics and that teardown runs. Report what passed, what was not verified, and which decisions should be revisited as the suite grows.

If adding network guards, verify that out-of-scope traffic is ignored, a late unexpected error still fails the test, and a one-shot expectation does not hide a second matching error. Check that missing and mismatched expectations fail, and cover GraphQL errors under HTTP 200 if GraphQL monitoring is enabled.
