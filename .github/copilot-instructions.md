# Repository AI Working Instructions

This repository is an AI-assisted engineering template focused on reducing unnecessary token consumption by expensive reasoning models.

## Core operating model

Route work by need and use the smallest capable agent for each stage.

1. Start normal engineering work with **Engineering Router** when the correct path is not already obvious.
2. Use **Context Scout** when repository discovery is needed or before expensive reasoning that depends on repository context.
3. Use a high-cost reasoning model only for architecture, difficult debugging, security-sensitive decisions, ambiguous cross-component behavior, migration trade-offs, or other work that genuinely benefits from deeper reasoning.
4. Use **Implementer** for routine implementation, tests, formatting, documentation updates, validation, and explicit Git housekeeping.
5. Use **Reviewer** to verify an existing implementation or diff before escalating back to deep reasoning.
6. Return to the high-cost reasoning model only when a genuinely difficult unresolved decision remains.

## Need taxonomy

Use these need codes consistently:

- `NEEDS_ROUTINE_IMPLEMENTATION`
- `NEEDS_CONTEXT_DISCOVERY`
- `NEEDS_DEEP_REASONING`
- `NEEDS_VALIDATION_OR_REVIEW`
- `NEEDS_USER_INPUT`

Task size alone does not justify expensive reasoning. Large but mechanical work belongs with Implementer.

When `NEEDS_DEEP_REASONING` is identified and repository context matters, Context Scout should prepare the smallest standalone task packet before the advanced model is used.

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

## Handoff discipline

Use native agent handoffs when available instead of asking the user to manually restate context between Copilot agents.

Do not auto-submit a handoff that could unexpectedly modify code unless the next action is clearly safe and already within the user's approved scope. Prefer `send: false` for implementation and fix handoffs so the user can inspect the prepared next step.

External reasoning models are not assumed to participate in VS Code handoffs. When the selected advanced model is external, provide a compact standalone prompt rather than broad repository context.

## Model independence

Astra is the initial high-cost reasoning model used to validate this workflow, but instructions, agents, and context files should remain model-agnostic wherever practical.

Do not introduce Astra-specific assumptions into shared architecture unless they are explicitly isolated and documented.

## Project context maintenance

Keep `.ai/PROJECT_CONTEXT.md` concise. Update it only with stable information that is likely to prevent repeated repository discovery in future tasks.

Do not turn it into a chronological work log or duplicate the README.
