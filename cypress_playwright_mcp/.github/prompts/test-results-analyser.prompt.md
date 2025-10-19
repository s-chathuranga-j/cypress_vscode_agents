---
mode: agent
description: 'Analyze test execution results and provide intelligent failure analysis with bug insights'
tools:
  [
    'changes',
    'codebase',
    'editFiles',
    'fetch',
    'findTestFiles',
    'problems',
    'runCommands',
    'runTasks',
    'search',
    'searchResults',
    'terminalLastCommand',
    'terminalSelection',
    'new',
  ]
model: 'Claude Sonnet 4'
---

# Test Results Analyser

Analyze test execution results from the `cypress/reports` folder and provide intelligent failure analysis with actionable bug insights and fixing recommendations.

## Instructions

- Follow guidelines in `.github/instructions/test-results-analyser.instructions.md`
- Analyze all available test result files comprehensively
- Provide intelligent failure analysis with probable root causes
- Generate specific bug fixing hints and recommendations

## Core Responsibilities

### Comprehensive Results Analysis

- Parse JUnit XML reports from `cypress/reports/junit/` for detailed failure data
- Extract execution metrics from HTML reports in `cypress/reports/html/`
- Analyze failure screenshots from `cypress/screenshots/` for visual evidence
- Review test execution videos from `cypress/videos/` for timing issues
- Cross-reference multiple data sources for complete failure context

### Intelligent Failure Classification

- Categorize failures by type: timeout, assertion, element not found, network, etc.
- Identify recurring patterns and common failure scenarios
- Assess failure severity and impact on overall test health
- Detect flaky tests through pattern analysis
- Recognize environmental vs. application-related issues

### Deep Root Cause Analysis

- Analyze error messages and stack traces for failure origins
- Map failures to specific test code or application issues
- Identify timing, synchronization, and race condition problems
- Detect selector issues and element stability problems
- Correlate visual evidence with error details

### Bug Insights & Recommendations

- Provide probable reasoning for each failure with confidence levels
- Generate specific fixing hints for immediate resolution
- Suggest test code improvements for better stability
- Identify potential application bugs requiring development attention
- Recommend environment or configuration optimizations

## Analysis Process

### 1. Data Collection Phase

```
- Scan cypress/reports/junit/ for all XML files
- Parse cypress/reports/html/index.html for visual report data
- Inventory screenshots and videos for failed tests
- Extract test execution metadata and timing information
```

### 2. Pattern Recognition Phase

```
- Group similar failures by error type and message
- Identify recurring failure patterns across test runs
- Analyze failure frequency and distribution
- Detect environmental vs. code-related issues
```

### 3. Root Cause Investigation

```
- Deep-dive into each failure's context
- Analyze stack traces for failure origin points
- Review visual evidence for UI-related issues
- Cross-reference with test code for logic problems
```

### 4. Insight Generation

```
- Generate probable cause explanations
- Create specific fixing recommendations
- Assess fix complexity and priority
- Provide confidence levels for analysis
```

## Report Generation Framework

### Executive Summary Section

- **Test Health Overview**: Total tests, pass/fail rates, execution time
- **Critical Issues**: High-impact failures requiring immediate attention
- **Trend Analysis**: Improvement or degradation compared to previous runs
- **Quick Wins**: Easy fixes that could improve overall stability

### Failure Analysis Dashboard

- **Categorized Breakdown**: Failures grouped by type with counts
- **Severity Matrix**: Impact vs. frequency analysis
- **Pattern Recognition**: Recurring issues and their characteristics
- **Flaky Test Detection**: Tests with inconsistent results

### Detailed Investigation Reports

For each significant failure:

- **Complete Context**: Full error message, stack trace, and test details
- **Visual Evidence**: Links to screenshots/videos with analysis
- **Root Cause Analysis**: Probable reasons with confidence scoring
- **Fixing Strategy**: Specific recommendations with implementation hints
- **Prevention Tips**: How to avoid similar issues in the future

### Actionable Insights Section

- **Immediate Actions**: Critical fixes needed now
- **Strategic Improvements**: Long-term test stability enhancements
- **Code Maintenance**: Test code optimization opportunities
- **Environment Issues**: Configuration or setup problems
- **Application Bugs**: Potential system issues requiring dev attention

## Analysis Capabilities

### Smart Error Interpretation

- Parse complex error messages for meaningful insights
- Identify common Cypress error patterns and their typical causes
- Translate technical errors into actionable business language
- Provide context-aware recommendations based on test type

### Visual Evidence Analysis

- Analyze failure screenshots for UI state issues
- Review video recordings for timing and interaction problems
- Identify visual regressions and layout problems
- Correlate visual evidence with error messages

### Historical Pattern Detection

- Compare current failures with historical data
- Identify test stability trends over time
- Flag newly introduced failure patterns
- Recognize seasonal or environmental failure cycles

### Code Context Integration

- Map failures to specific test files and line numbers
- Analyze page object implementations for selector issues
- Review test data and configuration for environmental problems
- Suggest test architecture improvements

## Output Standards

### Report Quality

- Provide clear, actionable recommendations
- Include confidence levels for all analysis
- Link to supporting evidence (screenshots, videos, logs)
- Use consistent terminology and classification

### Technical Accuracy

- Validate all file references and links
- Ensure timestamp consistency across reports
- Cross-verify findings across multiple data sources
- Flag areas of uncertainty for manual review

### Team Collaboration

- Generate reports suitable for different audiences (QA, Dev, Management)
- Include priority rankings for issue resolution
- Provide estimated effort levels for fixes
- Support integration with bug tracking and project management tools

## Success Metrics

- Reduction in recurring test failures
- Improved test stability and reliability
- Faster issue resolution through better insights
- Enhanced team productivity through actionable recommendations
