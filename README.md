# Agentic Engineering Template

A reusable repository template for reducing token consumption of high-cost, token-intensive AI models by preparing focused repository context, generating optimized task prompts, and delegating routine implementation to lower-cost agents.

The first target workflow is built around **Astra** as the high-cost reasoning model, but the design is intentionally model-agnostic. Astra can be replaced by any advanced reasoning model without changing the core workflow.

## The problem

Powerful reasoning models are valuable for architecture, difficult debugging, security-sensitive decisions, and cross-component analysis. They are also expensive in context and tokens when they spend time rediscovering a repository, reading unrelated files, performing routine edits, running basic checks, or handling Git housekeeping.

This template separates those responsibilities.

## Core workflow

```text
Developer request
      |
      v
Context Scout
(read + search only)
      |
      v
Reasoning Task Packet
      |
      v
High-cost reasoning model
(Astra initially)
      |
      v
Approved implementation plan
      |
      v
Implementer
(routine code changes + validation)
      |
      v
Review / tests / commit / push
```

The goal is not to use a weaker model for important decisions. The goal is to make sure expensive reasoning tokens are spent only where they add the most value.

## Design principles

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
    context-scout.agent.md
    implementer.agent.md
  copilot-instructions.md
.ai/
  PROJECT_CONTEXT.md
README.md
```

## Agents

### Context Scout

The Context Scout is a read-only reconnaissance agent. It converts a normal developer request into a compact, standalone task packet for a high-cost reasoning model.

It should:

- inspect only relevant parts of the repository,
- identify exact files and relationships,
- distinguish facts from assumptions,
- exclude irrelevant context,
- produce a concise prompt that can be sent to Astra or another reasoning model,
- never implement the requested change.

### Implementer

The Implementer receives an already-approved plan and performs routine repository work. It should not redesign the solution unless the implementation proves that the plan is impossible or unsafe.

Typical responsibilities include:

- code and infrastructure changes,
- tests and validation,
- formatting and linting,
- small documentation updates,
- reporting blockers back to the developer.

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
3. Give your normal task description to **Context Scout**.
4. Send only the generated **Reasoning Task Packet** to the selected high-cost reasoning model.
5. Return the resulting implementation plan to **Implementer**.
6. Run the relevant validation and review the resulting diff.
7. Use the high-cost model again only if a genuinely difficult decision or blocker remains.

## Initial target stack

None. This repository intentionally avoids coupling the workflow to AWS, Azure, Terraform, Python, JavaScript, or any other technology. Technology-specific instructions should live in repositories created from this template.

## Status

Early experimental template. The first real-world validation target is an Astra + GitHub Copilot workflow. The agent contracts and context format are expected to evolve based on measured token savings and task quality.

## What success looks like

A successful use of this template should reduce unnecessary context sent to expensive reasoning models while preserving or improving implementation quality. Over time, projects can measure:

- reasoning-model token usage per task,
- number of files passed to the reasoning model,
- repeated repository discovery,
- number of reasoning-model turns required,
- implementation rework after handoff.

The objective is simple: **reduce waste, not capability.**
