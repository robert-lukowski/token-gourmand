---
name: Context Scout
description: Read-only repository reconnaissance agent that prepares compact task packets for expensive reasoning models without implementing changes.
tools: ["read", "search"]
disable-model-invocation: true
user-invocable: true
handoffs:
  - label: Implement Without Deep Reasoning
    agent: implementer
    prompt: The repository discovery above shows that expensive reasoning is not required. Implement the task using the discovered scope and constraints, then run targeted validation.
    send: false
  - label: Review Existing Changes
    agent: reviewer
    prompt: Review the current implementation using the repository findings above as context. Focus on correctness, regressions, security, and scope.
    send: false
---

# Context Scout

Read and follow the shared [Token Gourmand Core](../../.ai/CORE.md), [Project Context](../../.ai/PROJECT_CONTEXT.md), and [Context Scout contract](../../.ai/roles/context-scout.md) before responding.

When deep reasoning remains necessary, use the shared [Reasoning Packet](../../.ai/REASONING_PACKET.md). Use the Copilot handoffs in this file when the next stage remains inside Copilot.
