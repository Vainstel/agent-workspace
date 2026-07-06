---
name: claudemd-author
description: How to write/fill CLAUDE.md files at the correct level of this workspace — org, product/team, and repo — with the fixed per-level section structure. Also covers CLAUDE.local.md (the personal, gitignored repo-level layer). Use when creating or (re)filling such a file from scratch. To refresh an existing file, use claudemd-actualize.
---

# Authoring CLAUDE.md by level

This workspace layers context: **workspace → org → product/team → repo**. Every piece of information lives at exactly **one** level, so an agent reading top-down finds the right depth at the right place and nothing rots from duplication.

> The **workspace** level (the root `CLAUDE.md`) is template-owned and rarely changes — edit it only on explicit request, and only its "Ground rules" / "Additional" sections.

## Before writing: read the neighbors

Always read the `CLAUDE.md` **one level above** and any **already existing below** your target level. You need to know what's already said to (a) not duplicate it and (b) push each detail to its proper level. If a detail you want to add already lives on another level — leave it there, or move it (never both places).

## Per-level structure

Each file = H1 + date stamp, then the level's **required** sections, then at most 1–2 of its **optional** sections *if genuinely useful*, then an optional final **`## Additional`** — a short bullet list of specific, slippery-but-important facts (e.g. a default local auth token, a feature quirk worth knowing on day one). Don't invent content to fill sections: a small org/product/repo legitimately yields a short file. Hard ceiling ~200 lines; most files should be far under.

### Org — `<org>/CLAUDE.md`

| Section | What goes in |
|---|---|
| **What this org is** *(required)* | 2–4 lines: the company/scope and the user's role/area in it. |
| **Products** *(required)* | Do **not** enumerate everything (agents `ls` for the live list). Mention only products needing context beyond their name — one line each — and how they relate if part of a bigger whole. |
| **Org conventions** *(required)* | Commit/ticket patterns (e.g. `[TICKET-123] Short message`), language, review flow, org-wide dos/don'ts. |
| **Shared infrastructure** *(optional)* | Systems spanning products: cloud accounts/tenants, SSO, shared data warehouses. |
| **Knowledge sources** *(optional)* | Where org docs live (Confluence space, wiki) and which MCP reaches them. |

### Product / team — `<org>/<product>/CLAUDE.md`

| Section | What goes in                                                                                           |
|---|--------------------------------------------------------------------------------------------------------|
| **What & why** *(required)* | Business framing: what the product does, why it exists, main functions. "Understand it in ~2 minutes." |
| **Repos** *(required)* | Map of the product's repos by name, one line each (what each is for).                                  |
| **How it fits together** *(required)* | Cross-repo/service communication, very general domains and data flow at role level.                    |
| **Deploy & infra** *(optional)* | Who does what (e.g. "chart = templates, infra = per-cluster values"), key external systems.            |
| **Docs & links** *(optional)* | Product-level external docs (Confluence pages, dashboards).                                            |

No code, class names, schedules, or deep tables here — that's repo level.

**Composite products.** When the product is a multi-repo composite (submodules / composite build / mono-root), there is no extra level: this same product file lives inside the composite root repo and additionally carries the *system map* — shared domain glossary, cross-service flow, build system & local infra, cross-cutting gotchas. Never duplicate a service's internals in it.

### Repo — inside each repository

| Section | What goes in |
|---|---|
| **What this repo is** *(required)* | 1–3 lines. If it's a standalone mono-project (no product level above), fold a short what/why in here. |
| **Structure & entry points** *(required)* | Package/module map, key classes/files, internal flow. |
| **Build / run / test** *(required)* | Commands, run profiles, JDK/tooling versions, important env vars. |
| **Config** *(optional)* | Pointers to the repo's own config files/sections (never the volatile values themselves). |
| **Gotchas** *(optional)* | Repo-specific traps. |

## CLAUDE.local.md — personal repo-level layer

The repo's `CLAUDE.md` is maintained by the team and lives in the repo's git — it's often generic or missing details only an individual developer needs. `CLAUDE.local.md` is the same kind of file, but **personal**: extra details and nuances tied to *this developer's machine and environment* — local run/e2e/infra setup that differs between people, personal dev tokens or creds that don't belong in the shared file (real secrets only as pointers to where they live).

Rules:
- **Repo level only**, and only next to an existing repo `CLAUDE.md` — it complements it, never replaces it. Shared-worthy facts you discover belong in the team file (propose them), not buried here.
- **Never tracked by git.** Before writing it, make sure that repo's own `.gitignore` covers `CLAUDE.local.md`; add the entry if missing.
- **No volatile content.** No TODOs, task statuses, current-work paths — that churns too fast to keep actualized. Same durable-facts bar as any CLAUDE.md.
- Same format: H1 + date stamp, short sections, size far under the ceiling.

## Exception: single-repo project directly under an org

A standalone mono-project may sit directly under an org with **no product layer** — allowed as an exception, don't invent a product wrapper for it. It gets only a repo-level CLAUDE.md with a short what/why folded in (see the repo table). `private/` uses this pattern freely: it may hold both multi-repo products and mono-projects side by side. In all cases derive levels from **actual nesting**, not fixed depth — walk the path, decide which levels exist, fill each accordingly.

## Cross-cutting rules (every level)

1. **One detail, one level.** Higher = broader/why; lower = narrower/how.
2. **Repo files are self-contained.** Repos get cloned standalone. Never reference another repo by file path or a sibling/parent `CLAUDE.md` by path — reference services/repos **by name**; external links are fine. Use repo-standalone commands (`./gradlew`, not `../gradlew :module:task`).
3. **No config-owned volatile data** (schedules, flags, batch sizes, secret values). Point at the config file/section instead.
4. **No historicity.** Current state only; no changelogs; delete stale content freely.
5. **Concise & projectable.** Bullets over prose walls; phrase facts so they generalize beyond one ticket. Don't write what an agent trivially derives from the code — if something derivable is still load-bearing, one short pointer, not a rehash.
6. **Language.** Repo-level content in English (match codebases); higher levels may use the user's working language.
7. **Verify, don't transcribe.** Read the actual code/repo; don't copy possibly-stale external docs. For multi-repo products, dispatch parallel research subagents (one per repo), then write the files yourself.
8. **Never blindly overwrite.** Read the existing file first — preserve the user's manual edits.
9. **"Read before working" reminder.** Each org/product file should remind agents to read the `CLAUDE.md` (+ `CLAUDE.local.md`) of every lower level a task touches.

## Date stamp (required)

Right under the H1:

```
*Actualized: YYYY-MM-DD*
```

Set to today when authoring. It's the baseline that makes future actualization cheap (see claudemd-actualize).

## Process

1. Identify the level(s) from the path (watch for the single-repo-under-org exception).
2. Read the neighbor CLAUDE.md files (above + below) and any existing target file.
3. Check the org's knowledge sources (MCP: Confluence/wiki). If no such MCP is connected, offer the user to activate/add one before you proceed — verified external docs make the result noticeably better; if they decline, continue with repo/code only.
4. Investigate the real repo/code — parallel subagents for multi-repo.
5. Write only level-appropriate sections; push other detail to its proper level.
6. Add the date stamp (today).
7. Self-check: no cross-repo paths, no volatile config data, no historicity, no duplication with neighbor levels, under the size ceiling.
