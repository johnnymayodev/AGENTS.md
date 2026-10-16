# Agent instructions

## Global Defaults

- Repo-specific `AGENTS.md` or `AGENTS.override.md` instructions override these defaults.
- Always respect the standards and conventions that already exist in a codebase; they take precedence over these defaults.
- User specific instructions override all of these.

## Branch policy

- Before making codebase changes, check the current branch. `main` is the only static branch; all work happens on short-lived feature branches created from the latest `main`.
- Never commit directly to `main`; feature branches merge into `main` only through pull requests.
- If currently on `main`, create a feature branch from it before editing. If on a branch other than `main` or this task's feature branch, switch to the task's feature branch (creating it from the latest `main` if needed) only if doing so is safe; if switching could disturb existing work, stop and ask.
- Releases come from tagged commits on `main`; never create release branches or release from untagged commits.

## Commit and push preferences

- Commit only when the user asks; otherwise leave changes uncommitted. When committing, put the requested changes on the task's feature branch; never commit directly to `main`. If switching to the task's feature branch could disturb existing work, stop and ask rather than committing on an unrelated branch.
- Never push automatically. A request to commit does not authorize a push; a request to open a pull request includes pushing the branch.
- Do not push directly to `main` or merge into `main` yourself; `main` changes only through pull requests.
- Keep commits small and limited to changes relevant to the request. Check the working tree and staged diff, stage specific files, and leave unrelated work untouched.
- Do not automatically commit when relevant checks fail or cannot run. Report the blocker and ask how to proceed. Never weaken tests or bypass hooks to make checks pass.
- Use a clear, descriptive commit subject and follow the repository's subject/body convention. Do not add `Co-authored-by` or other attribution trailers.
- Do not force-push or rewrite/rebase history.
- When pushing, push only the intended changes to the appropriate remote and branch. Ask first if the destination or scope is unclear, and report where the push went.

### #Pull Requests

- Open a PR only when the user asks; the rules in this section apply when they do.
- Make sure titles follow conventions from the repo. They should be simple and easy to understand. Conventional commit styles and projects that use them. If none then follow this style: 'fix(server): parse CLI version in update preflight'
- PR description should aim for simplicity. Open with a minimal, clear description of the problem. Follow up with how you solved it.
- Do not add any co authoring or mentions of models used.
- **Open a real PR, not a draft.** 
- **While the branch is local and unpushed, rebase it onto the base branch (usually `main`) before opening.** Once a branch is pushed, do not force-push; merge or rebase the base in and ask before rewriting published history.

## Security and external actions

- Never expose or commit secrets or credentials.
- Require authorization before deploying, publishing packages, or modifying production systems or data.

### Blast Radius

- Never touch production live databases unless explicitly told to. When a task is adjacent to any of them, name what you're about to do and ask before doing it.

## Software installation and downloads

- Do not install software or download files, installers, runtimes, binaries, SDKs, or CLI tools.
- Installing dependencies already declared by the project through its documented setup is allowed.
- Adding a project dependency requires prior user approval. Explain what it provides, why it is needed, and why existing dependencies or built-in APIs are insufficient before asking. Do not reimplement substantial, established functionality (such as schema validation) just to avoid proposing an appropriate dependency.
- After approval, make only the manifest and lockfile changes needed to add that dependency. This does not authorize unrelated dependency updates or installing optional extras.
- Do not install language runtimes, system packages, or global tools as a workaround for a missing tool.
- Do not run remote bootstrap or install scripts, or shell pipelines that download and execute scripts (for example, `curl ... | sh`).
- Before running project setup scripts that may install software beyond the project's declared dependencies, inspect what they do and ask for approval if their effects are unclear or broader than the documented setup.
- Do not change machine-wide configuration, services, shell profiles, `PATH`, permissions, or environment settings to support an installation.
- If required software is unavailable, use tools already present where practical; otherwise explain what's missing and ask how to proceed.

## Engineering practices

### Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

- State assumptions before implementing. If uncertain, ask.
- If multiple interpretations exist, identify them and ask when the choice could change scope or behavior; don't pick silently.
- Prefer a simpler approach when it meets the request, mention relevant tradeoffs, and push back when warranted.
- If something is unclear, name what's confusing and ask one concise question when the answer could change scope or behavior; otherwise state your assumption and proceed.
- Treat questions, reviews, and planning requests as read-only with respect to implementation files.
- Whenever asked to create or build a plan to fix something, write it to a descriptively named Markdown file in `.agents/plans/` at the project root, creating the directory if needed. Before finishing, ensure the directory is ignored by the project's Git ignore rules; if it is not already ignored, add `.agents/plans/` to the project-root `.gitignore`.

### Project guidance

- Before editing, read applicable repository instructions and relevant setup/contribution documentation.
- Use the project's documented build, lint, type-check, and test commands.
- Keep actual project-specific commands in each project's `AGENTS.md` rather than this shared file.

### Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- Make the smallest change that solves the request; don't add features beyond what was asked.
- Avoid speculative features, abstractions for single-use code, flexibility or configurability that wasn't requested, and error handling for impossible scenarios.
- If an implementation becomes disproportionately large, simplify it before continuing. If you write 200 lines and it could be 50, rewrite it.
- Ask: “Would a senior engineer say this is overcomplicated?” If yes, simplify.

### Surgical Changes

**Touch only what you must. Clean up only your own mess.**

- Touch only the files and lines needed for the request.
- Preserve unrelated work and existing behavior, formatting, and comments.
- Don't improve adjacent code, comments, or formatting; don't refactor things that aren't broken.
- Match the existing style, even if you'd do it differently.
- Remove imports, variables, or functions made unused by your changes. Don't remove pre-existing dead code; if you notice unrelated dead code, mention it instead.
- Every changed line should trace directly to the user's request.

### Multi-Agent

- Do not spawn sub agents or a multi agent panel for work a single agent finishes in one pass.
- When several agents do work in parallel, state file ownership upfront so they do not collide.

### Goal-Driven Execution

**Define success criteria and loop until verified.**

- Define a concrete success criterion for each change.
- Transform requests into verifiable goals: for validation, test invalid inputs; for a bug, first write a test that reproduces it; for a refactor, ensure tests pass before and after.
- For multi-step tasks, state a brief plan with a verification check for each step:
  ```
  1. [Step] → verify: [check]
  2. [Step] → verify: [check]
  3. [Step] → verify: [check]
  ```
- Add or update tests for behavioral changes and run the relevant tests and checks afterward.
- Report failures with their location, cause, and fix or next diagnostic.
- Before destructive actions, confirm the exact target and user intent.
- After three unsuccessful fixes, stop and identify the doubtful assumption instead of layering on more guesses.
- Summarize what changed, which checks actually ran and their results, and anything skipped or blocked. Never claim verification that was not performed.

These guidelines are working if they lead to fewer unnecessary changes, fewer rewrites caused by overcomplication, and clarifying questions before implementation rather than after mistakes.

## Preferences 

### Technology

- If not already specified in the project. I generally like to use the following technology: React, TypeScript, Vite, shadcn/ui with Base UI, Bun, Tailwind CSS, Hono, Drizzle ORM.

### Design Work

- Follow the project's design system, theme, or design tokens when one exists; these defaults apply only when it doesn't.
- Avoid continuously repainting CSS animations (pulse, shimmer, animated blur, spinners).
- Dark modes should be true black background with white primary text.
- The user's color scheme preference should always be enforced by default.

### TypeScript

- Avoid `any` unless there is no reasonable type alternative.

### Bun

- `Bun.serve()` supports WebSockets, HTTPS, and routes. Don't use `express` unless specified.
- `bun:sqlite` for SQLite. Don't use `better-sqlite3`.
- `Bun.redis` for Redis. Don't use `ioredis`.
- `Bun.sql` for Postgres. Don't use `pg` or `postgres.js`.
- `WebSocket` is built-in. Don't use `ws`.
- Prefer `Bun.file` over `node:fs`'s readFile/writeFile
- Bun.$`ls` instead of execa.