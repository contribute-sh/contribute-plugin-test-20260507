# Constitution
Version: 0.1.0

## Purpose
Build a small CLI to-do app in TypeScript. The MVP is a single-binary command-line tool that lets a user add tasks, list outstanding tasks, and mark them done. Storage is a JSON file in the user's home directory. The goal is a working MVP shipped end-to-end as a contributor smoke test for the Contribute coordinator + plugin path.

## Principles
- The MVP must be runnable end-to-end with `pnpm start -- add "...", list, done <id>`.
- Persist state to `~/.contribute-todo.json`. No databases, no servers, no network calls.
- Keep dependencies minimal. Use `commander` for arg parsing; nothing else from npm.
- Tests cover the three commands (add, list, done) end-to-end via `node --test`.
- TypeScript strict mode, no `any`, no `@ts-ignore`.

## Boundaries
- No external API integrations.
- No web UI. CLI only.
- No telemetry.
- Single TypeScript package — do not introduce a monorepo or multi-package layout.

## Quality Standards
- `pnpm typecheck` (`tsc --noEmit`) is clean.
- `pnpm test` (Node's built-in test runner) passes.
- `pnpm lint` is clean (eslint defaults).
- The `add`, `list`, `done` flow works in a fresh shell with an empty home dir.

## Stack
- Language: TypeScript
- Package manager: pnpm
- Test runner: node:test
- Lint: eslint
- Build: tsc

## Verification
- `pnpm typecheck`
- `pnpm test`
- `pnpm start -- add "buy milk" && pnpm start -- list` produces an entry, `pnpm start -- done <id>` removes it from `list`.

## Roadmap

### MVP
- Scaffold `package.json`, `tsconfig.json`, `src/index.ts`, `src/storage.ts`, `tests/cli.test.ts`.
- Implement `add <title>` — append a task with a generated id to `~/.contribute-todo.json`.
- Implement `list` — print outstanding tasks one per line, formatted `[id] title`.
- Implement `done <id>` — remove the task with that id, error if id is unknown.
- Add tests for each command using a temp HOME dir.

### Hardening
- Validate input (empty title, malformed id, corrupt JSON file).
- Handle concurrent writes safely.
- Add a `--storage <path>` flag to override the storage location.

### Polish
- Color the `list` output by age.
- Add `due <date>` field with relative-date display.
- Package as a global `npm install -g` binary.
