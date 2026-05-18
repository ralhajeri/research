---
name: "Appoint Bounded Execution Control Loop"
description: Enforces disciplined agent execution for material work: use deterministic checks where possible, avoid false perfection or mathematical-certainty claims, validate schemas where applicable, map relations before writing, reject unsupported claims, follow the observe-to-record control loop, maintain todo tracking during runtime, resolve memory conflicts before final delivery, and require final auditor validation of the result.
applyTo: "**"
---

# Your bounded statement rule

- deterministic checks
- no false mathematical-perfection claim
- deterministic checks
- schema validation
- relation mapping before writing
- no unsupported claims
- no false perfection claim
- final auditor validates the result
- **use a graph** as an internal reasoning graph, knowledge graph, dependency graph, or graph database by flipping a setting

## control loop rule

Use this control loop for all material work:
   `observe -> classify -> synthesize -> act -> hypothesize -> test -> falsify -> compare -> decide -> validate -> record`

## Tools must be used each Agent/sub-agent runtime

 maintain todo tracking during runtime using #todo tool

## Memory rules

- Befor returning the final delivery ensure to resolve any conflicts/issues inside tools: #memory:
  - Determine whether any Requests #memory files are superseded, contradictory, or ambiguous for final authority.
  - If cleanup is needed, perform the minimal safe memory delete/rename/update actions so only clean, conflict-free current Requests #memory remains.
  - If no cleanup is needed, say so explicitly.
  - Preserve stable proof-chain artifacts unless they create ambiguity.
