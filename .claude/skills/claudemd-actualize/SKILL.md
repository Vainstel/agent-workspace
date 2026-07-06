---
name: claudemd-actualize
description: How to refresh/actualize an EXISTING CLAUDE.md (any level) or CLAUDE.local.md against current reality — repo commits, code, and the org's knowledge sources (Confluence/wiki, Slack via MCP). A targeted update (remove stale, insert current truth), NOT a rewrite. To create/fill a file from scratch, use claudemd-author.
---

# Actualizing CLAUDE.md

Goal: bring an existing `CLAUDE.md` in line with how things are **today**. This is a **diff/update**, not a rewrite — preserve what's still correct, surgically remove what's outdated, add what's now true.

> Read claudemd-author first if unsure what belongs at each level (org, product, repo) — its per-level structure and cross-cutting rules apply here too.

## Sources of current truth

Bound the search window with the file's `*Actualized: YYYY-MM-DD*` stamp:

1. **Repo commits & code** — primary. `git log --since=<stamp>`: if the commit count is reasonable, go through them and read the changed areas. If there are too many to review individually, don't try — aggregate the touched paths instead (`git log --since=<stamp> --name-only` or `git diff --stat`), see which parts of the app changed most, and analyze the **current state** of those areas. Either way, anything the file names (class/file/flag/command) is a claim it exists — verify by grep/read.
2. **Org knowledge base** (Confluence/wiki via MCP) — current architecture, process, ownership.
3. **Chat** (Slack via MCP) — recent decisions, migrations, deprecations, incidents in the relevant channels.

If an MCP for a source isn't connected, offer the user to activate/add it first — it makes the actualization noticeably more reliable; if they decline, proceed with the remaining sources and never guess.

## Rules

1. **It's a DIFF, not a rewrite.** Keep correct content and structure; prefer targeted `Edit`s over full `Write`.
2. **Preserve manual edits.** Read the whole file first; only correct what is actually outdated.
3. **Remove outdated info outright** — no "previously X, now Y". The file reads as if always current.
4. **Verify before keeping.** Confirm every named artifact still exists; rename/move/delete as reality dictates.
5. **Re-check levels.** If a detail drifted to the wrong level, move it (never duplicate). Read the neighbor CLAUDE.md above/below to keep the one-detail-one-level invariant.
6. **Same cross-cutting rules as claudemd-author**: names not paths across repos, no volatile config data, concise, size ceiling, don't restate what's derivable from code.
7. **Flag the unverifiable.** No access to a source → tell the user instead of guessing.
8. **Fix ambiguity too.** Vague, contradictory or unactionable instructions you encounter are fair game to tighten.
9. **CLAUDE.local.md**: same diff approach, but sources are personal — git/code plus the user's own answers (no Confluence/Slack needed). Strip any volatile content that crept in (TODOs, task statuses, current-work paths) — it doesn't belong there (see claudemd-author).

## Date stamp

After actualizing, set the top line to today: `*Actualized: YYYY-MM-DD*`. If missing, add it under the H1.

## Process

1. Read the target file fully (stamp = baseline; note manual edits).
2. Gather current state: git log + changed code; knowledge base; chat (whatever MCP allows).
3. Build a short diff list: stale / missing / changed / unverifiable.
4. Apply targeted edits; keep structure and correct parts intact.
5. Update the stamp.
6. Report what changed and what you couldn't verify.
