# Project Context

Keep this file short. Its purpose is to prevent AI agents from repeatedly rediscovering stable project facts.

Delete instructional placeholder text as the repository becomes established.

## Purpose

Describe what this repository exists to deliver in 2-4 sentences.

## Architecture Summary

List only the major components and their relationships.

Example:

```text
Client -> API -> Application service -> Data store
                 |
                 -> External service
```

## Important Paths

Document only paths that frequently matter to engineering tasks.

| Path | Purpose |
| --- | --- |
| `src/` | Replace with project-specific purpose |
| `tests/` | Replace with project-specific purpose |

## Runtime / Platform

- Primary language/runtime:
- Infrastructure/platform:
- Package/build system:
- Infrastructure as Code:
- CI/CD:
- Primary deployment environment:

Remove fields that do not apply.

## Deployment Model

Describe the minimum stable deployment facts that agents repeatedly need to know, such as:

- deployment branches,
- environments,
- authentication mechanism,
- region or platform boundaries,
- promotion model.

Do not store credentials, access tokens, secrets, or sensitive environment values here.

## Engineering Constraints

List stable constraints that materially affect implementation.

Examples:

- avoid long-lived cloud credentials,
- preserve backwards compatibility for a public interface,
- infrastructure changes must remain declarative,
- production changes require pull-request review.

## Important Decisions

Record only decisions that would otherwise be repeatedly reopened by AI agents.

| Decision | Reason |
| --- | --- |
| Replace with a stable architectural decision | Brief rationale |

For detailed Architecture Decision Records, link to them instead of duplicating them here.

## Current State

Summarize the current meaningful project state in a few bullets. Keep this current, not historical.

## Known Boundaries

State areas that are intentionally outside the project's scope or should not be changed casually.

## AI Context Notes

- Prefer targeted repository discovery over broad scans.
- Treat this file as orientation, not proof. Verify task-critical facts against the current implementation.
- Do not paste this entire file into another model when only a subset is relevant.
- Update this file only when a stable fact changes.
