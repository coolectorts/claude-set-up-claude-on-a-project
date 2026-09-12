# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Starter Express API for the Claude Code course: an in-memory `users` resource plus a `/health` check, used as a real codebase to practice setting up `CLAUDE.md` and permission rules on.

## Commands

- `npm install` — install dependencies
- `npm run dev` — start the API with `node --watch` (auto-restarts on file changes), on `http://localhost:3000`
- `npm start` — start the API without watch mode
- `npm test` — run all tests (Node's built-in `node --test` runner + `supertest`)
- `node --test tests/users.test.js` — run a single test file
- `npm run lint` — run ESLint (`eslint:recommended`)
- CI (`.github/workflows/ci.yml`) runs `npm install`, `npm run lint`, then `npm test` on Node 22 for every push and PR — match that locally before pushing.

## Conventions

- CommonJS throughout (`require`/`module.exports`), not ESM — `.eslintrc.json` sets `sourceType: "script"`.
- Route handlers validate input and return early with `res.status(...).json({ error: "..." })`; they don't throw.

## Architecture

- `server.js` is the entry point: builds the `express` app, applies `express.json()`, and mounts `routes/users.js` at `/users` and `routes/health.js` at `/health`. It only calls `app.listen` when run directly (`require.main === module`), so `tests/` can `require("../server")` and drive it in-process with `supertest` without opening a real port.
- `routes/` holds one router file per resource; each exports an `express.Router()` mounted in `server.js`.
- `db/store.js` is a tiny in-memory array-based store standing in for a real database — state resets on every restart and starts seeded with two users.
- This repo is the exercise target for the course task itself: the goal is to add `CLAUDE.md` / `.claude/settings.json` / `NOTES.md`, not to change the app code in `server.js`, `routes/`, or `db/`.
