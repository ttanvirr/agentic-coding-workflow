# What is Agentic Coding?

Agentic coding is a software development approach in which you delegate coding tasks to an AI agent that can plan, write code, use tools, inspect results, and iterate toward a goal with varying levels of autonomy.

Instead of asking AI to generate a code snippet and manually integrating it yourself, you give an agent a task, provide the necessary context, and let it work through some or all of the development process.

For example, you might tell an AI coding agent:

> "Build a responsive React task manager with task creation, completion toggles, filtering, and persistent local storage."

The agent might then inspect your project, create components, install dependencies, implement functionality, run the application or tests, investigate errors, and revise its code.

You still guide the process, review its work, and decide whether the result is acceptable.

## 1. Traditional coding vs. AI-assisted coding vs. agentic coding

### Traditional coding

You analyze the requirements, design the solution, write the code, run it, debug errors, and maintain the application yourself.
You perform the development work manually.

### AI-assisted coding

You ask an AI assistant to explain a concept, generate a function, suggest a fix, or review a piece of code. You typically control the surrounding workflow and apply the changes yourself.
AI helps with individual development tasks.

### Agentic coding

You assign a goal to an AI agent that can work with your codebase and tools, make changes, execute commands, evaluate results, and continue working based on what happens.
AI handles a sequence of connected development tasks.

## How agentic coding works

An agentic coding workflow commonly follows this cycle:

1. `Understand the goal:` Interpret the task and available context.
2. `Plan:` Break the task into manageable steps.
3. `Act:` Read files, write code, and use tools.
4. `Evaluate:` Run tests, inspect output, and detect errors.
5. `Iterate:` Fix problems and refine the implementation.

Repeat as necessary until the goal is reached or the agent needs human input.

This is a conceptual workflow, not a requirement that every agent use these exact steps.

### Example

Suppose you're building a React application and ask an agent to add a task creation form.

1. `Understand:` The agent examines your existing React application and identifies how it is structured.
2. `Plan:` It determines which components and state management changes are needed.
3. `Act:` It creates or modifies the relevant files.
4. `Evaluate:` It runs available checks, such as a build or tests.
5. `Iterate:` If the build fails, it investigates the error and attempts a fix.

The key distinction is that the agent can use the results of its actions to determine what to do next, rather than requiring you to provide every next step.

## What makes an AI coding agent different?

An AI model alone is not necessarily a coding agent. Agentic coding generally combines several capabilities.
|Capability |Purpose|
|-----------|-------|
|AI model |Understands instructions and reasons about possible solutions.|
|Context |Provides relevant code, documentation, requirements, and project conventions.|
|Tools |Enables file access, terminal commands, code editing, tests, and other actions.|
|Planning |Breaks larger goals into smaller tasks.|
|Feedback loop |Uses command output, test results, and errors to guide subsequent actions.|
|Autonomy |Allows the agent to perform multiple steps without requiring approval for every step.|

Not every agent has all these capabilities. Some can edit files but cannot execute commands; others can use a terminal, browse documentation, run tests, and interact with external services.

## 4. What is the developer's role?

Agentic coding does not eliminate the developer's responsibilities. It changes how the work is performed.
As a developer, you remain responsible for:

- `Requirements`: Defining what should be built and what success means.
- `Architecture`: Establishing appropriate project structure and technical decisions.
- `Context`: Providing instructions, constraints, and relevant documentation.
- `Supervision`: Reviewing plans and approving sensitive actions when necessary.
- `Verification`: Checking correctness, security, accessibility, performance, and maintainability.
- `Accountability`: Deciding whether the final implementation is suitable for production.

An agent can produce code that builds successfully but still contains architectural flaws, security vulnerabilities, or incorrect business logic.

## What tools are used for agentic coding?

Some examples include:

- Gemini CLI
- Claude Code
- OpenAI Codex

# Agentic Coding Workflow

Suppose, we want to use Gemini CLI as a free agentic coding tool to build a React application. The workflow would look like this:

For a React project, first, we'd create the project manually using Vite, install the dependencies, initialize Git, and give Gemini CLI a few project instructions. Then let the agent help you build the application incrementally.
The idea is to establish a reliable foundation—not to do all the work before introducing the agent.

## 1. Set up the project foundation

For a new React + TypeScript project:

```bash
# Create the project
npm create vite@latest my-app -- --template react-ts

# Enter the project
cd my-app

# Install dependencies
npm install

# Initialize Git
git init
git add .
git commit -m "chore: initialize React project"
```

Run the application once before involving the agent:

```bash
npm run dev
```

## 2. Add project instructions for Gemini CLI

Gemini CLI supports a project-level `GEMINI.md` file containing persistent instructions. It loads that context when working in the project, so you don't have to repeat your conventions in every prompt.

Start with a short instruction file rather than an enormous specification.

`GEMINI.md`

```md
# Project Instructions

## Project Overview

- This is a React application built with TypeScript and Vite.
- Follow the existing project structure and conventions.

## Development Rules

- Inspect relevant files before making changes.
- For substantial tasks, propose a plan and wait for approval.
- Prefer small, focused changes over unnecessary refactoring.
- Reuse existing components and dependencies where appropriate.
- Do not install dependencies without explaining why they are needed.
- Do not overwrite working code or configuration without a reason.

## Code Quality

- Use TypeScript appropriately; avoid `any` unless justified.
- Follow existing formatting and linting rules.
- Handle loading, error, empty, and success states where relevant.
- Keep components focused and reusable.

## Verification

- Run the relevant lint, type-check, test, and build commands available in the project.
- Fix regressions caused by your changes.
- Never claim a check passed unless it was actually executed.
- Summarize changed files, verification results, and remaining concerns.

## Safety

- Never expose secrets or put credentials in source code.
- Ask before destructive commands or significant architectural changes.
- Do not commit or push changes without explicit approval.
```

## 3. Let Gemini inspect the project before building features

Install Gemini CLI:

```bash
npm install -g @google/gemini-cli
```

Start it from the project root:

```bash
gemini
```

Authenticate using one of the supported methods. Then give it an initial exploration task.

```
Inspect this React project and its GEMINI.md instructions.

Do not modify any files yet.

Explain:
1. The existing project structure and technology stack.
2. The scripts available in package.json.
3. The linting, type-checking, testing, and build capabilities already configured.
4. Any important missing development tooling.

Recommend the smallest set of improvements needed to make this project ready for reliable agentic development. Do not install anything or modify configuration yet. Explain the trade-offs and wait for my approval.
```

This first interaction is deliberately read-only in intent. Keep Gemini CLI's normal tool approval prompts enabled; the CLI supports approval modes and sandboxing for additional control.

## 4. Add quality checks before substantial development

The Vite React TypeScript starter already includes basic development tooling. Check the scripts in `package.json`:

```bash
npm run
```

Then verify the existing build and lint scripts:

```bash
npm run lint
npm run build
```

As the application grows, consider adding:

- Vitest + React Testing Library for component and application tests.
- TypeScript type checking as a separate verification step where needed.
- GitHub Actions to run checks automatically on pull requests.

Don't install all of these automatically. Ask Gemini to assess what's missing and recommend additions based on the project.
