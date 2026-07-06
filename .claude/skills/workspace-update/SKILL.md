---
name: workspace-update
description: Update/actualize this agent-workspace itself from its source template — pull improved skills, commands, agents, base instructions and docs while preserving all user content. Uses .workspace-meta.yml (template source + version) as the baseline. Use when the user asks to update the workspace, refresh its tooling, or sync with the template.
---

# Updating the workspace from its template

Goal: bring the workspace's **template-owned** assets up to the template's latest state without touching anything the user owns. This is a guided merge, not a blind overwrite.

## Ownership map

**Template-owned (candidates for update):**
- `.claude/skills/` — skills the template ships (`claudemd-author`, `claudemd-actualize`, `workspace-update`, …)
- `.claude/agents/` and `.claude/commands/` — template-shipped agents and their `init_*` commands
- Root `CLAUDE.md` (base rules), `README.md`, `docs/`
- The non-org sections of `.gitignore` (scratch/local-config/OS-noise), `.workspace-meta.yml`

**User-owned (NEVER touch):**
- All org/product/repo directories and their CLAUDE.md files; the per-org blocks in `.gitignore`
- `workflow/`, `tools/`, `TMP/`, `archive/`, `.mcp.json` (server choices), `.claude/settings*.json`
- Anything the user added themselves under `.claude/` (skills/agents/commands not present in the template)

## Process

1. **Baseline.** Read `.workspace-meta.yml`: `source` (template repo URL) and `template_version`.
2. **Fetch.** Shallow-clone the template into `TMP/workspace-update-<date>/` (full clone if tags are needed). Read the template's own `.workspace-meta.yml` → the new version. Same version and no diffs → report "already up to date" and stop.
3. **Diff per template-owned file.** Prefer a 3-way view when the template repo has a tag matching the local `template_version` (`v<version>`): old-template vs new-template vs local. Classify each file:
   - **unchanged** — template and local identical → skip.
   - **update** — template changed, local matches the old template (or was never edited) → safe to apply.
   - **new** — file exists only in the template → offer to add.
   - **removed** — template dropped it, local unmodified → offer to remove.
   - **conflict** — both the template and the user changed it → do NOT auto-resolve; show a short side-by-side of the differing sections and let the user pick (keep local / take template / merge manually with your help).
   Without a usable tag, treat every template-vs-local difference conservatively: anything that might be a user edit is a **conflict**, not an **update**.
4. **Confirm.** Present one compact table (file → classification → one-line what changed). Apply only after the user confirms — all or a subset. Never commit or push; leave changes for review.
5. **Finalize.** Update `.workspace-meta.yml`: set `template_version` to the new version. Delete the clone from `TMP/`.
6. **Report.** What was applied, what was skipped, unresolved conflicts, and anything the template's changelog/README says needs manual action.

## Rules

- User content is sacred: when in doubt whether a file is user-edited — ask, don't overwrite.
- Renames in the template (e.g. a skill renamed) = remove + add; point this out explicitly so the user's muscle memory doesn't break silently.
- If the template introduced new conventions (naming, structure), apply them only to template-owned assets; suggest — not perform — any migration of user content.
- The update itself must not change the behavior of the user's orgs/products in any way.
