# Cypress Framework - AI Coding Agent Instructions

## Architecture Overview

This is a **Cypress framework project with TypeScript** designed with standardized best practices. The framework follows the **Page Object Model (POM)** pattern and supports both UI and API testing with comprehensive reporting and accessibility testing.

### Key Components

- **`cypress/pages/`** - Page Object classes extending `BasePage` (exported as singletons)
- **`cypress/testbase/`** - Base classes (`BasePage.ts`, `ApiBase.ts`), API clients, and utilities (`Utils.ts`)
- **`cypress/tests/ui/`** - UI tests using page objects and session management
- **`cypress/tests/api/`** - API tests using custom commands, API clients, and typed request/response models
- **`cypress/testbase/apiClients/`** - Typed API client wrappers for DRY, maintainable API interactions
- **`cypress/testbase/modals/`** - TypeScript interfaces for API requests and responses
- **`subscription-api/`** - Standalone Node.js test API server (port 3000) with Swagger documentation

## Critical Development Patterns

### Page Object Model Implementation

**Always use singleton pattern for page objects:**

```typescript
// InventoryPage.ts
class InventoryPage extends BasePage {
  url = '/inventory.html';
  title = () => cy.get('[data-test="title"]'); // Arrow functions for selectors
}
export default new InventoryPage(); // Singleton export
```

**Page methods:** CRITICAL - Only create methods for complex actions that involve multiple steps or complex logic. DO NOT create methods for simple actions like `click()`, `type()`, `get()`, `should()`, `clear()`, etc. These simple element interactions and assertions should be done directly in the test files.

**Element locators:** PRIORITY ORDER for element selection:

1. First: `data-test`, `data-cy`, `data-qa`, `data-test-id` attributes
2. Second: Unique `id` attributes
3. Third: Stable CSS selectors (class names that won't change)
4. Last resort: XPath or complex CSS selectors

**ALWAYS inspect the actual DOM structure first** and use the most reliable selector available.

**Session management pattern for authentication:**

```typescript
// In page objects, use createSession() for efficient login
LoginPage.createSession(); // Creates cy.session for reuse
```

### API Testing Architecture

**Use typed API clients for maintainable API interactions:**

```typescript
// ProductTypesClient.ts - Centralized, typed API operations
class ProductTypesClient {
  public createProductType(
    details: ProductTypeRequests.CreateProductTypeRequest
  ): Cypress.Chainable<object> {
    return cy.sendApiRequestDef(
      apiEndpoints.productTypes,
      HttpMethod.POST,
      Cypress.env('bearerToken'),
      StatusCodes.CREATED,
      details
    );
  }
}
export default new ProductTypesClient(); // Singleton pattern
```

**Custom commands from `ApiBase.ts` with flexible status validation:**

```typescript
// For API calls with automatic status validation
cy.sendApiRequestDef(apiEndpoints.productTypes, HttpMethod.GET, 'null', StatusCodes.OK);
// Supports multiple expected status codes
cy.sendApiRequestDef(endpoint, method, token, [StatusCodes.OK, StatusCodes.NOT_FOUND]);
// With query params (cy-api and cy.request variants)
cy.sendApiRequestWithParams(endpoint, method, token, StatusCodes.OK, { page: '1' });
cy.sendApiRequestDefWithParams(endpoint, method, token, StatusCodes.OK, { page: '1' });
// With file attachments
cy.sendApiRequestWithAttachment(endpoint, method, token, StatusCodes.CREATED, resourceId, filePath);
```

**Bearer token and Swagger schema helpers:**

```typescript
cy.getAccessToken().then(token => Cypress.env('bearerToken', token));
cy.fetchSwaggerSchema().then(schema => {
  /* use for AJV validations */
});
```

**Comprehensive API architecture layers:**

- **`testbase/apiEndpoints.ts`** - Centralized endpoint definitions with parameterized URLs
- **`testbase/apiClients/`** - Typed client wrappers (ProductTypesClient, CustomersClient, etc.)
- **`testbase/modals/requests/`** - TypeScript interfaces and builder classes for request payloads
- **`testbase/modals/responses/`** - TypeScript interfaces for response validation

### Test Organization Conventions

**UI tests structure (follow Arrange-Act-Assert pattern):**

- Use `{ tags: ['@smoke', '@inventory-page'] }` for test filtering (via `@cypress/grep`)
- Import page objects: `import InventoryPage from '../../pages/InventoryPage';`
- Use `before()` hooks for session setup, `beforeEach()` for page navigation
- Keep tests independent - no shared state between tests

**API tests structure:**

- Store dynamic data in Cypress env: `Cypress.env('productTypeId', response[0].id)`
- Use enums from `support/Enums.ts` for HTTP methods and status codes
- Leverage API clients and request builder modals for readable, maintainable test code
- Use namespaced request/response models for type safety

**Naming conventions:**

- UI tests: `<feature>_tests.spec.ts` (e.g., `login_page_tests.spec.ts`)
- API tests: `<endpoint>_api_tests.spec.ts` (e.g., `product_type_api_tests.spec.ts`)

**Accessibility testing patterns:**

```typescript
// Strict: fail on critical/serious/moderate
const a11yOptionsStrict = {
  runOnly: ['wcag2a', 'wcag2aa'],
  includedImpacts: ['critical', 'serious', 'moderate'],
};
before(() => {
  cy.visit('/');
  cy.injectAxe();
});
it('no a11y violations', () => {
  cy.checkAccessibility(undefined, a11yOptionsStrict);
});

// Lenient: collect but do not fail
const a11yOptionsLenient = { runOnly: ['wcag2a', 'wcag2aa'], includedImpacts: [] };
cy.checkAccessibility(undefined, a11yOptionsLenient);
```

## Essential Workflows

### Running the Complete Test Suite

1. **Start subscription API (for API tests):**

   ```bash
   cd subscription-api && npm install && npm start
   ```

2. **Run tests:**
   ```bash
   npm run cypress:open    # Interactive mode
   npm run cypress:run     # Headless mode
   npx cypress run --env grepTags=@smoke  # Tag filtering via @cypress/grep
   ```

### Framework Setup (for new projects)

```bash
npm install cypress-bootstrap
npx cypress-bootstrap-setup  # Auto-scaffolds project structure
```

## Configuration Architecture

**Multi-environment support via `cypress.config.ts`:**

- Base URL: `process.env.CYPRESS_BASE_URL || 'https://www.saucedemo.com/'`
- API URL: `process.env.API_BASE_URL || 'http://localhost:3000'`
- Test isolation disabled: `testIsolation: false` for session efficiency
- Viewport: 1920x1080; timeouts: 30s default/response/request
- Spec pattern: `**/tests/**/*.spec.ts`; fixtures: `cypress/testdata`

**Plugins and support setup (`cypress/support/e2e.ts`):**

- Import: `cypress-plugin-api`, `cypress-real-events`, `cypress-mochawesome-reporter/register`, `cypress-map`, `cypress-wait-until`, `cypress-axe`, `wick-a11y`, `cypress-recurse`
- Register grep: `const { register } = require('@cypress/grep'); register();`
- Optionally suppress app exceptions: `Cypress.on('uncaught:exception', () => false);`

**Grep and typing:**

- Add `@cypress/grep` to `tsconfig.json` `types`
- Provide `cypress/support/types/cypress-grep.d.ts` with `tags?: string | string[]`
- Use `{ tags: [...] }` in tests for filtering

## Integration Points

### Reporting Pipeline

- **cypress-multi-reporters** configured via `reporter-config.ts`
- **Mochawesome** for HTML+JSON reports (`cypress/reports/html`)
- **JUnit XML** for CI/CD (`cypress/reports/junit/results-[hash].xml`)

### Code Quality Pipeline

- **Prettier** auto-formatting via Husky pre-commit hooks
- **Lint-staged** runs formatting on staged files only
- Commands: `npx lint-staged`, `npm run format`, `npm run format:check`

### Utility Functions

**Available in `testbase/Utils.ts`:**

```typescript
import Utils from '../../testbase/Utils';
const randomId = Utils.generateRandomIdNumber(); // 7-digit ID
const randomString = Utils.generateRandomString(10); // Custom length string
const guid = Utils.generateGuid(); // UUID v4
```

**Custom Cypress commands (`cypress/support/commands.ts`):**

- `valueIsNotNull`, `valueIsNull`, `slowDownType`, `recursiveType`, `seeOption`, `getCypressEnvVariable`
- Include typings in `cypress/support/index.d.ts`
- Configure AJV formats for schema validation as needed

## Completion Criteria

Before considering a task complete, ensure:

1. All requested functionality has been implemented
2. Tests are executable and properly configured
3. Code follows established patterns and conventions
4. Documentation is updated where necessary
