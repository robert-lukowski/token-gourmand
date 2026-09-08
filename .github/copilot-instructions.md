# Repository AI Working Instructions

This repository is an AI-assisted engineering template focused on reducing unnecessary token consumption by expensive reasoning models.

## Core operating model

Use the smallest capable agent for each stage of work.

1. Use **Context Scout** to inspect the repository and prepare a compact task packet for difficult reasoning work.
2. Use a high-cost reasoning model only for architecture, difficult debugging, security-sensitive decisions, ambiguous cross-component behavior, or other work that genuinely benefits from deeper reasoning.
3. Use **Implementer** or another lower-cost coding agent for routine implementation, tests, formatting, documentation updates, and repository housekeeping.
4. Return to the high-cost reasoning model only when implementation reveals a genuinely difficult unresolved decision.

## Context efficiency

- Do not scan the whole repository by default.
- Start from the user's request and discover context progressively.
- Prefer exact file paths, symbols, diffs, and small relevant excerpts over broad context dumps.
- Do not include generated files, dependency trees, build output, large logs, or unrelated documentation unless required.
- When logs are necessary, keep the smallest excerpt that preserves the failure and its useful surrounding context.
- Reuse `.ai/PROJECT_CONTEXT.md` for stable project facts instead of rediscovering them repeatedly.
- Clearly distinguish repository evidence from assumptions and unknowns.

## Change discipline

- Preserve the scope approved by the user or reasoning plan.
- Avoid unrelated refactors.
- Prefer minimal, reviewable changes.
- Validate changes with the narrowest relevant tests first, then broader validation when justified.
- Inspect the final diff before declaring work complete.
- Do not redesign an approved solution during routine implementation unless evidence shows that the plan is impossible, unsafe, or incorrect.

## Model independence

Astra is the initial high-cost reasoning model used to validate this workflow, but instructions, agents, and context files should remain model-agnostic wherever practical.

Do not introduce Astra-specific assumptions into shared architecture unless they are explicitly isolated and documented.

## Project context maintenance

Keep `.ai/PROJECT_CONTEXT.md` concise. Update it only with stable information that is likely to prevent repeated repository discovery in future tasks.

Do not turn it into a chronological work log or duplicate the README.
