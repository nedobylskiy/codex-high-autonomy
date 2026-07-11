# Codex High Autonomy Rules For TypeScript Projects

This file extends the base Codex high-autonomy workflow for TypeScript projects.

If you use this template, follow both:
- the general operating model from the root template
- the TypeScript-specific implementation rules in this file

If you are a new agent, read these files in order:
- `agents.md`
- `.secrets.md` when secrets, credentials, or external integrations may matter
- `tasks.md`
- `latestStatus.md`
- `lastsPoints.md`
- `stop.md` if it exists and contains a real stop record

## Scope

These rules are written for Codex working on Node.js and TypeScript projects.

## Language Rules

- User-facing communication must stay in the user's language.
- Internal working notes may switch to a more token-efficient language when helpful.
- The selected user language and internal context language must always be recorded in this file.

### Current Language Selection

- User communication language: `<set-user-language-here>`
- Internal context language: `<set-internal-context-language-here>`

When the language choice changes for a task, update this file.

## Core Stack Rules

1. Use Node.js as the runtime.
2. Always create and maintain a `tsconfig.json`.
3. For web services, use Express.
4. API structure should follow a controller-based MVC-style layout using `tsoa` where suitable.
5. If a database is needed, use Sequelize.
6. If the database grows beyond a trivial local setup, treat PostgreSQL as the default full database target.
7. For local PostgreSQL testing on Windows, the project may use [nedobylskiy/pgsql-portable-win64-bins](https://github.com/nedobylskiy/pgsql-portable-win64-bins).
8. For image manipulation, use `jimp` with async/await flows.
9. For classic crypto exchange integrations, use `ccxt`.
10. For Telegram integrations, use either the direct Telegram API or `node-telegram-bot-api`.
11. For SSH integrations, use `ssh2`.
12. For config loading, prefer `dotenv` or JSON config files.
13. For Swagger UI exposure, use `swagger-ui-express`.

## TypeScript Implementation Style

- Prefer async/await end to end.
- Split code into small, readable classes or services, but do not over-fragment the codebase into unnecessary micro-classes.
- Keep typing strong and intentional.
- Use interfaces and explicit types for data contracts, especially for API input and output.
- Avoid loosely typed request and response payloads when building HTTP APIs.

## Stage 0: Request Preparation

Before active implementation starts, the agent should perform a Stage 0 preparation pass.

At this stage, the agent should:

1. Analyze the user's task.
2. Build a list of things that may be required to complete it.
3. Ask the user for missing inputs, access, files, credentials, environments, examples, or constraints that may be needed.

For TypeScript projects, this may include:

- API schemas or example payloads
- database connection details
- external service credentials
- expected environment variables
- deployment targets
- sample files or test fixtures

## Recommended Project Structure

For HTTP projects, prefer a structure close to:

```text
src/
  index.ts
  server.ts
  authentication.ts
  controllers/
  modules/
  types/
  routes.ts
tsconfig.json
tsoa.json
swagger.json
```

Guidance:

- `src/index.ts` should bootstrap configuration and start the app.
- `src/server.ts` should create the Express app, register middleware, register routes, and expose Swagger when enabled.
- `src/controllers/*Controller.ts` should define API endpoints through `tsoa` decorators.
- `src/routes.ts` should be treated as generated output from `tsoa`, not hand-edited business logic.
- `src/modules/` should hold services, integrations, and database-related logic.
- `src/types/` should hold shared TypeScript types and API contracts.

## Express Plus TSOA Pattern

For web APIs, the preferred pattern is:

1. Build the HTTP app with Express.
2. Define endpoint classes in `src/controllers`.
3. Use `tsoa` decorators such as `@Route`, `@Get`, `@Post`, `@Response`, `@SuccessResponse`, and `@Security`.
4. Generate OpenAPI spec and route bindings through `tsoa`.
5. Mount generated routes into the Express app.
6. Expose Swagger UI using `swagger-ui-express`.

This pattern is preferred because it keeps:

- routing declarative
- controllers discoverable
- OpenAPI generation consistent
- API typing closer to the controller contract

## Database Rules

- Use Sequelize whenever relational persistence is required.
- Keep database initialization isolated in a dedicated module or service.
- Keep models separate from controllers.
- Prefer PostgreSQL as the default serious database target.
- Use SQLite only for lightweight local development when appropriate, not as the default assumption for production-grade systems.

## Config And Secrets

- Keep runtime configuration in `.env`, JSON config files, or both.
- Document project-level secrets in `.secrets.md`.
- Keep `.secrets.md` in `.gitignore`.
- Never commit real secret values to public repositories.

## Task Start Rules For TypeScript Projects

When starting a new major TypeScript task, the agent should verify:

1. Stage 0 is completed and the likely required user-provided inputs are requested.
2. `package.json` exists or is created.
3. `tsconfig.json` exists and matches the project direction.
4. `tsoa.json` exists when the project exposes a typed HTTP API with controller-based routing.
5. `.secrets.md` exists if secrets or third-party integrations are involved.
6. `.secrets.md` is listed in `.gitignore`.
7. `tasks.md`, `latestStatus.md`, and `lastsPoints.md` are updated before work grows large.

## Execution Rules

The agent should continue working until one of these states is reached:
- the task is fully solved
- tests and checks are completed
- an unrecoverable blocking issue is found
- a practical timeout is reached and explicit user instruction is needed to resume

## Git Rules

1. Every project should be a Git repository.
2. Important milestones should be committed as separate commits.
3. Each commit message should clearly describe what was captured.
4. `.secrets.md` must stay ignored by Git unless the user explicitly wants a private-repository workflow that commits it.

## Instructions For A Newly Attached Agent

If you join this project after context loss or in a new thread:

1. Read `agents.md`.
2. Read `.secrets.md` if the task may depend on secrets, credentials, or external services.
3. Read `latestStatus.md`.
4. Read `lastsPoints.md`.
5. Read `tasks.md`.
6. Check `stop.md`.
7. Inspect `package.json`, `tsconfig.json`, and `tsoa.json` when working on the runtime or HTTP layer.
8. Inspect Git history if recent decisions need to be reconstructed.
9. Resume the active task instead of rebuilding context from scratch.
