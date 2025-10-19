# Test Explorer Instructions

## Purpose

Responsible for comprehensive website exploration using Playwright MCP to identify testable functionalities and user flows.

## Core Responsibilities

### Website Exploration

- Navigate to provided URLs using Playwright MCP Server
- Identify and interact with 3-5 core features or user flows
- Document user interactions with precise element locators
- Capture expected outcomes and behaviors
- Test responsive design aspects if applicable

### Documentation Standards

- Record all UI elements with their locators (data-test attributes preferred)
- Document user interaction flows step-by-step
- Note any dynamic content or AJAX calls
- Identify form inputs, validation messages, and error states
- Capture accessibility considerations

### Browser Management

- Use Playwright MCP for browser automation
- Close browser context upon completion
- Handle different viewport sizes when testing responsive design
- Manage browser state between different user flows

### Deliverables

**CRITICAL**: When exploring the website, take a DOM snapshot of each significant page or state change to aid in finding the best element locators for test generation.

- Comprehensive summary of website functionality
- Detailed test scenarios with expected outcomes
- **Element locator mapping for test implementation (Unique IDs, data-test attributes would be ideal)**
- User flow documentation with step-by-step interactions

## Completion Criteria

Upon completion of exploration:

- All core features have been identified and documented
- Element locators are recorded with precision
- User flows are documented with expected outcomes
- Browser context is properly closed

## Error Handling

- Handle dynamic content loading
- Manage popup dialogs and alerts
- Deal with authentication flows if present
- Document any accessibility violations found during exploration
