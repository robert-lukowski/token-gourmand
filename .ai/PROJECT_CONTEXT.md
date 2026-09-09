# Project Context

## Purpose

Token Gourmand is a reusable, model-neutral repository template for routing AI-assisted engineering work by need. It concentrates advanced reasoning on difficult decisions while keeping discovery, implementation, and review focused and context-efficient.

## Architecture

```text
Tool adapter -> shared .ai core and project context -> role contract
                                                    |
Context Scout -> standalone reasoning packet -> optional external reasoning model
```

The canonical workflow is `Engineering Router -> Context Scout -> Implementer -> Reviewer`, with shorter routes allowed by the need taxonomy.

## Important Paths

| Path | Purpose |
| --- | --- |
| `.ai/` | Model-neutral workflow, role contracts, reasoning packet, and project context. |
| `.github/agents/` | GitHub Copilot custom-agent profiles, tools, and native handoffs. |
| `.github/copilot-instructions.md` | Repository adapter for GitHub Copilot. |
| `AGENTS.md` | Repository adapter for OpenAI Codex. |
| `CLAUDE.md` | Repository adapter for Claude Code. |

## Engineering Constraints

- Repository content is technology-neutral Markdown with no runtime dependencies.
- Shared workflow rules belong under `.ai/`; adapters stay lightweight and tool-specific.
- Preserve the need codes and the existing Copilot custom-agent routing model.
- External reasoning models receive compact standalone packets instead of broad repository context.

## Known Boundaries

- The template does not install models, grant model access, manage credentials, or select subscriptions.
- It does not provide a CLI, generator, or cross-provider orchestration infrastructure.
- Adapter capabilities depend on the host product; shared role transitions remain conventions where native handoffs are unavailable.

## AI Context Notes

- Treat this file as orientation and verify task-critical facts against current files.
- Update it only when a stable project fact changes.
- Include only relevant excerpts when preparing external reasoning context.
