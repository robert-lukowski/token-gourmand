# Agentic Engineering Template

A reusable repository template for reducing token consumption of high-cost, token-intensive AI models by preparing focused repository context, routing work by need, generating optimized reasoning prompts, and delegating routine implementation to lower-cost agents.

The first target workflow is built around **Astra** as the high-cost reasoning model, but the design is intentionally model-agnostic. Astra can be replaced by any advanced reasoning model without changing the core workflow.

## The problem

Powerful reasoning models are valuable for architecture, difficult debugging, security-sensitive decisions, and cross-component analysis. They are also expensive in context and tokens when they spend time rediscovering a repository, reading unrelated files, performing routine edits, running basic checks, or handling Git housekeeping.

This template separates those responsibilities.

## Core workflow

```text
Developer request
      |
      v
Engineering Router
(classifies the primary need)
      |
      +--> NEEDS_ROUTINE_IMPLEMENTATION --> Implementer
      |
      +--> NEEDS_VALIDATION_OR_REVIEW ----> Reviewer
      |
      +--> NEEDS_CONTEXT_DISCOVERY -------> Context Scout
                                             |
                                             +--> routine -> Implementer
                                             |
                                             +--> deep reasoning
                                                      |
                                                      v
                                             Reasoning Task Packet
                                                      |
                                                      v
                                             High-cost reasoning model
                                             (Astra initially)
                                                      |
                                                      v
                                             Approved plan
                                                      |
                                                      v
                                                  Implementer
                                                      |
                                                      v
                                                   Reviewer
```

The goal is not to use a weaker model for important decisions. The goal is to make sure expensive reasoning tokens are spent only where they add the most value.

## Need codes

Agents use explicit need codes so routing decisions remain visible and auditable:

- `NEEDS_ROUTINE_IMPLEMENTATION` — the change is clear and mechanical.
- `NEEDS_CONTEXT_DISCOVERY` — the relevant implementation or scope must first be discovered.
- `NEEDS_DEEP_REASONING` — a material architecture, security, state, migration, cross-component, or difficult root-cause decision remains.
- `NEEDS_VALIDATION_OR_REVIEW` — implementation exists and should be checked.
- `NEEDS_USER_INPUT` — a required product, policy, environment, or business decision cannot be derived safely.

A large task is not automatically a deep-reasoning task. Large but mechanical work should remain with the routine implementation path.

## Handoffs

VS Code custom agents support handoffs between agents. This template uses them for the workflow stages that can remain inside Copilot, including:

- Router -> Context Scout,
- Router -> Implementer,
- Router -> Reviewer,
- Context Scout -> Implementer when deep reasoning is unnecessary,
- Implementer -> Reviewer,
- Reviewer -> Implementer for focused fixes.

The selected external high-cost reasoning model is intentionally not hard-coded. When Context Scout returns `NEEDS_DEEP_REASONING`, it also creates the compact standalone prompt that should be sent to Astra or another reasoning model.

## Design principles

- **Route before spending.** Classify the task before using an expensive model.
- **Reason first, implement second.** Expensive models should primarily resolve difficult decisions, not perform repository housekeeping.
- **Progressive context discovery.** Read only the files needed for the current task instead of loading the repository broadly.
- **Model agnostic.** The workflow must not depend on Astra-specific behavior.
- **Delegate routine work.** Implementation, tests, documentation updates, formatting, commits, and pushes should normally go to lower-cost agents such as GitHub Copilot.
- **Explicit scope.** Every reasoning request should state objective, relevant files, constraints, risks, unknowns, and definition of done.
- **No blind context dumping.** Large logs, entire documentation trees, and unrelated source files should not be passed to a reasoning model by default.
- **Evidence over assumptions.** Repository facts must be separated from inferred or missing information.

## Repository structure

```text
.github/
  agents/
    engineering-router.agent.md
    context-scout.agent.md
    implementer.agent.md
    reviewer.agent.md
  copilot-instructions.md
.ai/
  PROJECT_CONTEXT.md
README.md
```

## Agents

### Engineering Router

The default entry point. It performs lightweight triage and selects the smallest capable workflow based on the primary need.

### Context Scout

A read-only reconnaissance agent. It discovers the minimum repository context, decides whether expensive reasoning is justified, and produces a compact reasoning task packet only when necessary.

### Implementer

Executes a clear or approved plan, performs targeted validation, and handles routine Git housekeeping when explicitly requested. It should not redesign settled architecture.

### Reviewer

Checks an existing implementation or diff for correctness, regressions, security, required behavior, and unnecessary scope. It does not edit files and can hand focused findings back to Implementer.

## Project context

`.ai/PROJECT_CONTEXT.md` is deliberately short. A repository created from this template should keep it updated with stable information that would otherwise need to be rediscovered repeatedly, such as:

- project purpose,
- architecture summary,
- important directories,
- deployment model,
- engineering constraints,
- important decisions,
- current project state.

It is not intended to become a second README or a dump of the whole architecture. Its purpose is to save context tokens.

## Suggested usage

1. Create a new repository from this template.
2. Fill in `.ai/PROJECT_CONTEXT.md` with the minimum stable project context.
3. Start normal engineering requests with **Engineering Router**.
4. Follow the recommended handoff.
5. If Context Scout returns `NEEDS_DEEP_REASONING`, send only its generated reasoning prompt to the selected advanced model.
6. Return the approved plan to **Implementer**.
7. Hand the result to **Reviewer**.
8. Use the high-cost model again only if a genuinely difficult unresolved decision remains.

## Initial target stack

None. This repository intentionally avoids coupling the workflow to AWS, Azure, Terraform, Python, JavaScript, or any other technology. Technology-specific instructions should live in repositories created from this template.

## Status

Early experimental template. The first real-world validation target is an Astra + GitHub Copilot workflow. The agent contracts, need taxonomy, routing, and context format are expected to evolve based on measured token savings and task quality.

## What success looks like

A successful use of this template should reduce unnecessary context sent to expensive reasoning models while preserving or improving implementation quality. Over time, projects can measure:

- reasoning-model token usage per task,
- number of files passed to the reasoning model,
- repeated repository discovery,
- number of reasoning-model turns required,
- percentage of tasks completed without expensive reasoning,
- implementation rework after handoff.

The objective is simple: **reduce waste, not capability.**
