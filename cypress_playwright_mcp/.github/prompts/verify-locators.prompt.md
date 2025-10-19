---
description: 'Re-run Playwright MCP on selected cases to refresh locators'
tools: ['playwright', 'codebase', 'runCommands']
model: Claude Sonnet 4
---

Given the case IDs (comma-separated), re-run the steps from `artifacts/test-cases.json`, validate/repair selectors, and update `artifacts/locators.json`. Do not modify Cypress tests; only fix the selector map.
