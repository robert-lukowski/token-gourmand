# Token Gourmand Core

This file is the model-neutral source of truth for the Token Gourmand engineering workflow. Tool-specific adapters may explain how a particular agent consumes this core, but they must not redefine it.

## Operating Principle

Optimize for engineering quality per token spent. Use advanced reasoning where it materially improves a decision, and keep repository discovery, routine implementation, validation, documentation, and explicit Git housekeeping with the smallest capable engineering agent.

Cost does not override correctness. A smaller context is useful only when it still contains the evidence required to make a sound decision.

## Workflow

The shared workflow is:

`Engineering Router -> Context Scout -> Implementer -> Reviewer`

The Router may take a shorter path when the primary need is already clear:

- `Engineering Router -> Implementer` for routine implementation.
- `Engineering Router -> Reviewer` when an implementation or diff already exists.
- `Engineering Router -> Context Scout` when repository discovery or repository-grounded deep reasoning is required.
- `Engineering Router -> User` when a required decision cannot be derived safely.

Context Scout sends a clear task directly to Implementer. If a difficult decision remains, Context Scout prepares a compact standalone reasoning packet. The selected advanced reasoning model resolves that decision and returns an implementation plan. Implementer executes the plan, and Reviewer verifies the result. Material routine findings return to Implementer; only a genuinely unresolved difficult decision returns to advanced reasoning.

Use native handoffs when the host supports them. Otherwise, treat these roles as explicit stages in the current agent session and preserve the relevant outputs between stages.

## Need Taxonomy

Classify work with exactly one primary need. Secondary needs may be recorded when useful.

### `NEEDS_ROUTINE_IMPLEMENTATION`

The requested outcome or approved plan is clear and does not require a material architecture, security, state, migration, or cross-component decision. This includes straightforward edits, mechanical refactoring, focused tests, documentation changes, formatting, and explicitly requested Git housekeeping.

Next role: Implementer.

### `NEEDS_CONTEXT_DISCOVERY`

The objective is understandable, but the relevant implementation location, dependency path, current behavior, or affected scope is not yet known.

Next role: Context Scout.

### `NEEDS_DEEP_REASONING`

A material decision remains that benefits from advanced reasoning, such as an architecture trade-off, security or authorization boundary, difficult root-cause analysis, distributed state or concurrency problem, cross-component design, or migration with meaningful trade-offs.

When repository context matters, Context Scout must prepare the reasoning packet before the advanced model is used. Do not send the raw request or broad repository context.

### `NEEDS_VALIDATION_OR_REVIEW`

An implementation, plan result, or diff already exists and the primary job is to check correctness, regressions, security, required behavior, validation, and scope.

Next role: Reviewer.

### `NEEDS_USER_INPUT`

A required product, policy, environment, or business decision cannot be derived safely from repository evidence. Use this only after reasonable discovery cannot resolve the question.

Next step: request the smallest decision needed from the user, then route again.

Task size alone never justifies `NEEDS_DEEP_REASONING`. Large but mechanical work remains routine implementation.

## Context Efficiency

1. Start with the user request.
2. Read `.ai/PROJECT_CONTEXT.md` when its stable facts are relevant.
3. Search for exact paths, symbols, resources, workflows, or error strings.
4. Read only the relevant sections and follow only dependencies needed to understand the task.
5. Stop discovery when the task can be described and completed accurately.

Prefer exact paths, symbols, diffs, and small relevant excerpts. Exclude generated files, dependency trees, build output, large logs, vendored code, and unrelated documentation unless they are essential. When a log is needed, preserve only the failure and useful surrounding context.

Separate repository evidence from assumptions and unknowns. Treat project context as orientation rather than proof, and verify task-critical facts against the current implementation.

## Routine Implementation and Advanced Reasoning

Implementer owns execution of a clear request or approved plan, including focused repository inspection, code and documentation changes, directly affected tests, targeted validation, and reporting. It must not reopen settled architecture merely because another design is possible.

An advanced reasoning model owns only the difficult decision described in a compact packet. It should return a recommendation and a concrete implementation plan. It should not rediscover the repository, perform routine edits, run basic checks, format files, or handle Git operations unless the task explicitly makes one of those actions part of the difficult decision.

No specific model is required. Astra, GPT-5.6, or another suitable reasoning model may fill this role when the user has access to it.

## Escalation Rules

Escalate only when current evidence exposes one of the following:

- an approved plan conflicts materially with repository state;
- a security, authorization, state, or compatibility boundary must change unexpectedly;
- required information cannot be derived safely;
- a cross-component or migration decision is absent from the plan;
- validation disproves an architectural assumption;
- a difficult root cause remains after focused investigation.

Do not escalate routine implementation failures that can be fixed directly. Do not use advanced reasoning for cosmetic review findings or theoretical risks unsupported by the task.

An escalation must contain the exact blocker, repository evidence, affected paths or symbols, the decision required, relevant constraints, unknowns, and the smallest context needed to decide. Use `.ai/REASONING_PACKET.md` for `NEEDS_DEEP_REASONING`.

## Change and Validation Discipline

- Preserve the scope approved by the user or reasoning plan.
- Prefer the smallest coherent and reviewable change.
- Avoid unrelated refactoring and speculative abstractions.
- Do not broaden permissions, interfaces, or dependencies without explicit justification.
- Run the narrowest relevant validation first, then broader checks only when justified.
- Inspect the result before declaring completion.
- Report validation truthfully, including failures and checks that were not run.
- Commit or push only when the user explicitly requests it.

## Project Context

`.ai/PROJECT_CONTEXT.md` stores concise, stable project facts that would otherwise be rediscovered repeatedly. It should cover purpose, major architecture, important paths, runtime or platform, durable constraints, important decisions, current state, and known boundaries only when those facts are useful.

Keep it current and project-specific. Do not turn it into a second README, a chronological work log, a complete architecture document, or a copy of this workflow. Never store secrets in it. When preparing external context, include only the portions relevant to the decision.

## Role Contracts

Before performing a stage, read its contract:

- [Engineering Router](roles/engineering-router.md)
- [Context Scout](roles/context-scout.md)
- [Implementer](roles/implementer.md)
- [Reviewer](roles/reviewer.md)

When no role or path has already been selected, start with Engineering Router. If a user explicitly selects a role or provides an approved implementation plan, honor that entry point.
