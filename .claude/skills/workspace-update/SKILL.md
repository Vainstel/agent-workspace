---
name: workspace-update
description: "STUB — not implemented yet. Update/actualize this agent-workspace itself from its source template: pull improved skills, commands, agents and base instructions while preserving user content. Uses .workspace-meta.yml (template source + version) as the baseline."
---

# Workspace update (stub)

Placeholder for the workspace self-update flow. Planned behavior:

1. Read `.workspace-meta.yml` — template `source` and `template_version` are the baseline.
2. Fetch the template repo and diff its `.claude/` assets (skills, commands, agents), root `CLAUDE.md` rules and helper files against the local ones.
3. Apply improvements **without clobbering user content**: user's orgs/products, `workflow/`, `tools/`, `.mcp.json` server choices and any local edits stay untouched; conflicts are shown to the user, not auto-resolved.
4. Bump `template_version` in `.workspace-meta.yml`.

Until implemented: tell the user this skill is a stub and offer the manual path — compare with the template repo and cherry-pick updates.
