# tools

*Actualized: 2026-07-06*

Small standalone scripts useful to the user or to agents — utilities that don't belong to any product. Examples of what lives here: a query runner for a data warehouse with local creds baked in (so no heavyweight MCP is needed), one-off admin helpers, environment switchers.

Rules:
- One subdirectory (or single file) per tool, with a one-line usage comment at the top of each script.
- Scripts here MAY hold personal local credentials **only if this workspace repo stays private**; never mirror such scripts to shared repos.
- Agents: check here before writing a new helper — it may already exist.
