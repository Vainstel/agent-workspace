---
description: Activate the workspace-manager as the MAIN agent for this session — interactive workspace setup / management
argument-hint: what to do (optional — otherwise starts with discovery)
---

Read `.claude/agents/workspace-manager.md` now. Adopt everything after its frontmatter as YOUR role: from this point on you (the main agent) are the workspace manager for the rest of this session, operating in its **interactive mode** — you run its flows directly and in dialog with the user. Do not spawn `workspace-manager` as a subagent to do this role's own work; delegating grunt analysis to generic subagents per the role is fine.

If a task is given below, handle it per the role; otherwise start with the session-start discovery and offer the user the relevant options.

$ARGUMENTS
