# NOTES.md

## CLAUDE.md choices

I kept the description to one line, plus three short sections: Commands (`npm run dev`/`start`/`test`/`lint`, and how to run a single test file with `node --test tests/users.test.js`), Conventions (CommonJS modules and the early-return `{ error }` response pattern — both real, observable rules, not generic advice), and Architecture (the `server.js` entry point, one router per resource, the in-memory `db/store.js`, and why tests can `require("../server")` directly instead of needing a running dev server).

I left out a file-by-file inventory of `routes/` and `db/` — that's obvious from opening the directory — and any note about the app's business logic, since there isn't any beyond the sample `users` resource. I also didn't restate anything from `.eslintrc.json` that ESLint already enforces automatically; CLAUDE.md should cover what a session can't infer from running the linter itself. Nothing sensitive exists in this repo to accidentally include.

## Permission rules

I added an allow rule for `npm test` and `npm run lint` since both are read-only checks I run constantly and neither can modify files or state. I added a deny rule for `Read(./.env)` and for `Bash(git push --force:*)`, and an ask rule for `Bash(git push:*)`.

Without the `.env` deny rule, nothing stops a future session from reading real secrets into context the moment it's debugging something env-related — once a secret is in context it can leak into a commit message, an error paste, or a shared transcript. Without the force-push deny rule, a session could overwrite a collaborator's commits on a shared branch with no easy way back short of reflog recovery.
