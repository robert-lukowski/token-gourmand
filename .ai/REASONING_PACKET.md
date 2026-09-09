# Reasoning Packet

Use this format only when Context Scout assigns `NEEDS_DEEP_REASONING`. The packet must stand alone because the selected reasoning model may not have repository access, tool access, or earlier conversation history.

Keep the packet compact. Include the evidence needed to decide the issue, not a repository inventory. Prefer exact paths and symbols; include short source or log excerpts only when paths alone do not preserve the relevant meaning.

## Packet Format

```markdown
# REASONING TASK PACKET

## Objective
State the engineering outcome.

## Decision Required
Ask the precise architecture, security, state, migration, cross-component, or root-cause question that remains unresolved.

## Relevant Facts and Evidence
List verified repository facts and the evidence that supports them. Include essential small excerpts when the model cannot reason from a path or symbol alone.

## Relevant Paths and Relationships
List only the files and symbols needed for the decision, followed by the important dependency, call, data, workflow, or configuration relationships.

## Constraints and Non-goals
State user constraints, compatibility requirements, fixed decisions, and work intentionally outside scope.

## Options and Trade-offs
List known viable options and their established trade-offs. Write `Not established.` when discovery did not identify credible options.

## Assumptions and Unknowns
Separate assumptions from facts and identify missing information that affects confidence.

## Risks
List concrete risks supported by the task and evidence.

## Definition of Done
State observable completion conditions for the final implementation.

## Requested Output
Request:
1. a recommended decision and concise rationale;
2. rejected alternatives and material trade-offs;
3. assumptions or unknowns that must be resolved;
4. a concrete implementation plan organized by path or component;
5. targeted validation criteria.

Do not ask the model to inspect the repository or perform routine implementation, formatting, basic validation, or Git housekeeping.
```

## Packet Result

Return the reasoning model's accepted decision and implementation plan to Implementer. If the response depends on an unresolved product or policy choice, assign `NEEDS_USER_INPUT`. If it lacks repository evidence that Context Scout can obtain, assign `NEEDS_CONTEXT_DISCOVERY` and produce a revised packet afterward.
