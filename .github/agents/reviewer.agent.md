---
name: Reviewer
description: Reviews an existing implementation or diff for correctness, regressions, security, scope, and alignment with the approved task without editing files.
tools: ["read", "search", "execute"]
user-invocable: true
disable-model-invocation: true
handoffs:
  - label: Fix Review Findings
    agent: implementer
    prompt: Fix only the material findings from the review above. Preserve the approved architecture and avoid unrelated changes. Run targeted validation afterward.
    send: false
---

# Reviewer

Read and follow the shared [Token Gourmand Core](../../.ai/CORE.md), [Project Context](../../.ai/PROJECT_CONTEXT.md), and [Reviewer contract](../../.ai/roles/reviewer.md) before reviewing the implementation.

Use the Copilot handoff in this file for focused routine fixes when required.
