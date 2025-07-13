🔄 Project Awareness & Context
Always read PLANNING.md at the start of a new conversation to understand the project's architecture, goals, style, and constraints.

Check TASK.md before starting a new task. If the task isn’t listed, add it with a brief description and today's date.

Use consistent naming conventions, file structure, and architecture patterns as described in PLANNING.md.

Use nvm or your preferred Node.js version manager (e.g., fnm, volta) to ensure the correct Node.js version is used. Run npm install (or yarn install / pnpm install) whenever starting work or pulling new changes.

🧱 Code Structure & Modularity
Never create a file longer than 500 lines of code. If a file approaches this limit, refactor by splitting it into modules or helper files.

Organize code into clearly separated modules, grouped by feature or responsibility.
For agents/services, this looks like:

agent.ts - Main agent definition and execution logic

tools.ts - Tool functions used by the agent

prompts.ts - System prompts

Use clear, consistent imports (prefer relative imports within packages).

Use dotenv and config() for environment variables.

🧪 Testing & Reliability
Always create unit tests for new features (functions, classes, routes, etc.) using a framework like Jest or Mocha/Chai.

After updating any logic, check whether existing unit tests need to be updated. If so, do it.

Tests should live in a __tests__ or tests folder mirroring the main app structure.

Include at least:

1 test for expected use

1 edge case

1 failure case

✅ Task Completion
Mark completed tasks in TASK.md immediately after finishing them.

Add new sub-tasks or TODOs discovered during development to TASK.md under a “Discovered During Work” section.

📎 Style & Conventions
Use TypeScript as the primary language.

Follow common TypeScript/JavaScript style guides (e.g., Airbnb, Google) and format with ESLint and Prettier.

Use Zod or Joi for data validation.

Use Express.js, Fastify, or NestJS for APIs and TypeORM, Sequelize, or Prisma for ORM/ODM if applicable.

Write JSDoc comments for every function using the TSDoc style (or similar):

/**
 * Brief summary.
 *
 * @param {string} param1 Description.
 * @returns {number} Description.
 */
function example(param1: string): number {
    // Function body
    return 0;
}

📚 Documentation & Explainability
Update README.md when new features are added, dependencies change, or setup steps are modified.

Comment non-obvious code and ensure everything is understandable to a mid-level developer.

When writing complex logic, add an inline // Reason: comment explaining the why, not just the what.

🧠 AI Behavior Rules
Never assume missing context. Ask questions if uncertain.

Never hallucinate libraries or functions – only use known, verified Node.js packages.

Always confirm file paths and module names exist before referencing them in code or tests.

Never delete or overwrite existing code unless explicitly instructed to or if part of a task from TASK.md.
