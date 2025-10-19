# Visual Studio Code Custom Agent/Chat Modes for Cypress Test Generation

Generate Cypress tests using a VS Code Custom Agent powered by a Playwright MCP server.

This repository provides the scaffolding for wiring up a VS Code Custom Agent to a local MCP server so the agent can analyze your codebase and produce Cypress test cases. The intent is to keep the setup lightweight and editor-first.

## Overview

- **Editor**: Visual Studio Code with the Custom Agents feature enabled
- **Protocol**: Model Context Protocol (MCP)
- **Goal**: Let an agent generate or refine Cypress tests using context from your project

## Prerequisites

- VS Code (Insiders or a build that supports Custom Agents)
- A local runtime to host an MCP server (Node.js or Python, depending on your server implementation)
- Cypress installed in the target project where tests will be generated

## Repository Layout

- `cypress_playwright_mcp/`: Placeholder for the MCP server implementation (e.g., Playwright-powered analysis and resources)
- `README.md`: This file

## Setup

1. Clone the repo:
   ```bash
   git clone <your-repo-url>
   cd vscode_agents_cypress_pl_mcp
   ```
2. Copy the .github folder inside cypress_playwright_mcp folder if you intend to use Cypress with Playwright MCP
3. Paste it in your test project
4. Check if the VS Code shows the custom agents

   **Note:** Adjust the agents to your implementation if you are not following the architecture of the cypress-bootstrap framework.

## License

Add your license here (e.g., MIT) if you intend to open-source this repository.
