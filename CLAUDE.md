# Workspace

*Actualized: 2026-07-06*

This directory is the **entry point** for all work: every organization, product and repository lives under it. This file is the top (workspace) level of a four-level context chain.

## Levels

1. **Workspace** — this file. Base agent rules, independent of any company.
2. **Org** — `<org>/CLAUDE.md`. One directory per organization. `private/` (personal projects) is also an org.
3. **Product / team** — `<org>/<product>/CLAUDE.md`. A group of related repos.
4. **Repo** — `CLAUDE.md` inside each repository (own git; not tracked by this workspace repo).

Exception: a standalone single-repo project may sit **directly under an org** with no product layer — its repo-level CLAUDE.md folds in the short what/why.

A repo may also have a **`CLAUDE.local.md`** next to its CLAUDE.md: the personal, env-specific extension of the team-maintained file (local setup nuances, personal dev creds) — always gitignored, never committed. If present, read it together with the repo's CLAUDE.md.

## RULE #1 — read the CLAUDE.md files on your task path

Before doing **anything** in a task, read every `CLAUDE.md` (and `CLAUDE.local.md`, if present) on the path from here down to where you'll work: workspace → org → product → repo. This is the single most important rule in this workspace. Skipping a level means working blind — conventions, gotchas and access details live exactly one place each, so no single file is enough on its own. If a task touches several repos, read the chain for **each** of them. Re-check this rule whenever you `cd` somewhere new mid-task.

## Discover, don't enumerate

Lists of orgs/products/repos are deliberately **not** maintained here — they'd rot. Discover what exists with `ls` at each level; each level's CLAUDE.md mentions only the parts that need explanation beyond their name (e.g. a repo that is part of a bigger whole).

## Auxiliary directories (workspace level only)

- `workflow/` — the **operator's** notes for directing the agent (task briefs in `workflow/task/`, accumulated notes in `workflow/notes/`). Do not enter or mention it unless the user points you to a specific file. **One exception — task progress logs.** When working on a specific ticket, check whether `workflow/task/<TICKET>/` exists. If it does, read `progress_log.md` there to see where the task currently stands — and **only** that file: anything else in the directory may be stale, open it only when the user explicitly points to it. If the user gave you a ticket and no such directory exists, create it with a `progress_log.md` and keep that log briefly updated as the work progresses.
- `TMP/` — scratch. Use freely, create per-task subdirs. When asked to write a doc with no location given or script or report, put it here.
- `archive/` — retired projects and materials. Stay out.
- `tools/` — small scripts useful to the user or to you (see `tools/CLAUDE.md`).

## Ground rules for agents

- **Stay inside the workspace.** Never read or search files outside this directory unless the user explicitly points you to a path or gives permission first.
- **Never commit or push unless explicitly asked.** The user often reviews the diff first.
- **Docs have no history.** When updating any doc or CLAUDE.md: current state only, no "was X, now Y"; delete stale content freely. (Exception: docs explicitly meant as progress logs.)
- **Be brief with the user.** Answers and reports to the point; expand only when asked. Short instructions beat long essays.
- **Make answers easy to scan.** Format for the eye: short paragraphs, lists and code blocks where they help, key things highlighted — never a wall of text.
- **Check `.claude/skills` first.** If a skill covers the task (especially the `claudemd-*` skills for context files), use it instead of improvising. Suggest useful skills/MCP when they'd clearly help.
- **Don't write into CLAUDE.md what an agent trivially derives from code.** Only durable, load-bearing facts.

## Managing this workspace

Structure work (init org/product, integrate a repo, audit) → the **`workspace-manager`** agent — it's the expert for this. Context files (incl. `CLAUDE.local.md`) → the skills `claudemd-author` / `claudemd-actualize`.
This workspace repo tracks only: this file, org- and product-level `CLAUDE.md` files, `.claude/` assets, and `tools/`. Repository contents are ignored via a per-org block in `.gitignore`:
When adding an org, copy this block. If a product directory *is itself a git repo* (multi-repo composite), ignore the whole directory instead — its CLAUDE.md is tracked inside that repo.

## Additional

- `.mcp.json` is tracked and ships with default servers (docker, context7, idea, datadog, atlassian, github, playwright). For tokens prefer env-var references (`${VAR}`); literals are OK only while this repo stays private — never push them to a shared remote.
- Reminder: RULE #1 above is not optional. Read the CLAUDE.md chain before working.
