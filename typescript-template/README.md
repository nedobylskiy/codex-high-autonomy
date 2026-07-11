# TypeScript Template

This template extends the base Codex high-autonomy workflow for TypeScript projects built on Node.js.

It is intended for services and applications where the project should follow a consistent stack and architecture instead of choosing libraries ad hoc for every task.

## Stack Defaults

- Runtime: Node.js
- Language: TypeScript
- HTTP framework: Express
- API controller layer: `tsoa`
- Database ORM: Sequelize
- Primary full database target: PostgreSQL
- Local Windows PostgreSQL option: [nedobylskiy/pgsql-portable-win64-bins](https://github.com/nedobylskiy/pgsql-portable-win64-bins)
- Image manipulation: `jimp` in async style
- Classic crypto exchange integrations: `ccxt`
- Telegram integrations: direct Telegram Bot API or `node-telegram-bot-api`
- SSH integrations: `ssh2`
- Config loading: `dotenv` or JSON config files
- Swagger UI: `swagger-ui-express`

## Architectural Direction

The TypeScript rules here are based on a practical Express + `tsoa` structure similar to the one used in `Billions.works/payout-processor`.

That means:

- Express should own the app bootstrap and middleware wiring
- controllers should live in a dedicated `src/controllers` layer
- `tsoa` should generate routes and OpenAPI artifacts
- services should contain business logic
- models and database access should stay separated from controllers
- API request and response types should be explicitly typed

## Important For Codex

Codex should start with [`agents.md`](./agents.md) in this folder before creating or extending a TypeScript project from this template.
