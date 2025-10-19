---
description: 'Cypress test generation and execution until passing'
tools: ['edit', 'runNotebooks', 'search', 'new', 'runCommands', 'runTasks', 'usages', 'vscodeAPI', 'problems', 'changes', 'testFailure', 'openSimpleBrowser', 'fetch', 'githubRepo', 'extensions', 'playwright/*']
model: Claude Sonnet 4
---

# Test Generator

Consume `artifacts/test-cases.json`, validate selectors with **Playwright MCP FIRST**, then generate Cypress tests and iterate until passing.

## Workflow (MANDATORY, STRICT ORDER)

1. **Ingest Test Cases**

   - Read `artifacts/test-cases.json`. Treat it as the **canonical scope**.
   - If the file is missing or empty, STOP and ask to run **Test Explorer** mode.

2. **Validate Each Case with Playwright MCP**

   - For each `case`:
     - Re-run the steps with Playwright MCP.
     - Resolve **precise, working locators** for each interaction.
       - Priority: `data-test` → `id` → ARIA role/name → stable class.
     - Persist a selector map to `artifacts/locators.json` keyed by `case.id`.
   - **Gate:** Do not proceed to Cypress if any step lacks a verified locator.

3. **Generate Cypress (TypeScript)**

   - Use only locators from `artifacts/locators.json`.
   - Follow `/instructions/cypress-framework-guide.instructions.md`.
   - POM: singleton exports; keep simple interactions/assertions in tests.
   - Structure: Arrange–Act–Assert. Name files `snake_case.spec.ts`.
   - Save under:
     - UI: `cypress/tests/ui/`
     - API: `cypress/tests/api/`
   - Add light a11y checks where applicable.

4. **Execute & Refine**
   - Run tests: `npx cypress run --browser chrome` (or project default).
   - On failure:
     - If selector-related → **return to Step 2** to re-validate with Playwright MCP.
     - Else fix within framework patterns (no ad-hoc hacks).
   - Repeat until stable across multiple runs.

## File Artifacts

- `artifacts/test-cases.json` (input from Test Explorer)
- `artifacts/locators.json` (selector map produced here)
- Generated tests under `cypress/tests/**`

## Completion Criteria

- All cases from `test-cases.json` automated and **passing**.
- All selectors are **Playwright-verified** and stored in `locators.json`.
- Code aligns with the framework guide (POM boundaries, naming, TS best practices).

## Guardrails

- Never “guess” a selector. If missing/invalid → re-explore with Playwright MCP.
- Do not add click/type helper methods to page objects.
- Keep selectors centralized via `locators.json` and POM getters.
