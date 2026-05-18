---
name: "Appoint Agent MD Instructions"
description: "Instructions for authoring custom agent files (*.agent.md) in Appoint AI plugins."
applyTo: "**/*.agent.md"
---

<!-- @format -->

# Custom Agent File Instructions (`*.agent.md`)

These instructions apply to all `*.agent.md` files in the Appoint AI plugin
ecosystem. Authority boundaries are derived from the Appoint doctrine in
`docs/02-Orchestrator Worker Agent.md`, which defines a Parent Orchestrator /
Child Orchestrator / Worker model as an agent-architecture governance pattern
(not a native VS Code state specification).

## Naming Convention

- File name: `appoint-<name>.agent.md` (kebab-case)
- Agent `name` in frontmatter: `Appoint<Name>` (PascalCase)
- Example: `appoint-orchestrator.agent.md` → `name: AppointOrchestrator`

## AGENT_KIND Classification

Select the `AGENT_KIND` before authoring any agent file. Authority level
determines routing rules, tool allowance, and delegation scope.

| AGENT_KIND | Purpose | Routing rule | Delegation |
| --- | --- | --- | --- |
| `PARENT_ORCHESTRATOR` | Owns full mission, global scope, final merge, final validation | Explicit routing first by artifact kind, path, and workstream; semantic only for discovery | May invoke child orchestrators and workers via explicit bounded contracts |
| `CHILD_ORCHESTRATOR` | Owns one bounded workstream, local merge, local evidence | Explicit parent assignment first; semantic inside assigned scope only | May invoke workers for SRP slices within assigned boundary |
| `WORKER` | Executes one assigned task, produces evidence and structured handoff | Explicit assignment only | Must not invoke subagents without explicit parent authorization |

> **Routing rule**: More authority means less semantic guessing. Control must
> be explicit. Execution must be assigned. Completion must be proven.

## Required Frontmatter Fields

```yaml
---
name: Appoint<Name>                          # PascalCase, starts with "Appoint"
description: >
  One to three sentences describing what this agent does and when to use it.
tools:                                       # Least-privilege tool list per AGENT_KIND
  - search/codebase
  - read/file
model:                                       # Preferred models (ordered by priority)
  - Claude Sonnet 4.5 (copilot)
  - GPT-5.2 (copilot)
---
```

## Official Frontmatter Field Policy

Only officially supported `.agent.md` frontmatter fields may be used. Repo
policy adds further restrictions beyond what the platform documents.

| Field | Status | Notes |
| --- | --- | --- |
| `name` | Supported | Required. PascalCase, starts with `Appoint`. |
| `description` | Supported | Required. Drives auto-loading. Max 1024 chars. |
| `tools` | Supported | Required. Apply least-privilege per AGENT_KIND. |
| `model` | Supported | Optional. Prefer `Claude Sonnet 4.5 (copilot)`. |
| `agents` | Supported | Optional. Lists sub-agents this agent may invoke. |
| `user-invocable` | Supported | Optional. Default: `true`. Workers should be `false`. |
| `disable-model-invocation` | Supported | Optional. Default: `false`. |
| `hooks` | Supported | Optional. Agent-scoped lifecycle hooks. |
| `handoffs` | Supported | Optional. Suggested next steps after response. |
| `target` | **REPO-FORBIDDEN** | Do not add `target` to Appoint templates. This is a repo-hardening decision, not a claim that the platform forbids it elsewhere. |
| `github.permissions` | **EVIDENCE_REQUIRED** | Do not implement until an official VS Code or GitHub custom-agent source proves this exact field for the target runtime. Mark as `EVIDENCE_REQUIRED` until proven. |

## Body Content Requirement: 5W1H

Every `.agent.md` body must contain a `## 5W1H` section. Do not invent
5W1H frontmatter keys — this information belongs in the Markdown body only.

```markdown
## 5W1H

| Field | Value |
| --- | --- |
| Who | Who owns or invokes this agent and who receives its outputs |
| What | What bounded capability this agent provides |
| When | When to activate this agent (entry conditions) |
| Where | Where this agent may read or write (file and path boundaries) |
| Why | Why this agent exists (the governance outcome it protects) |
| How | How this agent routes, delegates, validates, and records |
```

## Per-AGENT_KIND Tool Policy

Apply the minimum tools needed for the role. Do not grant an orchestrator tool
set to a worker, or vice versa.

### PARENT_ORCHESTRATOR — allowed tools

| Tool | Use |
| --- | --- |
| `search/codebase` | Discovery and planning only |
| `read/file` | Read artifacts for context |
| `agent` | Invoke named child orchestrators and workers |

Forbidden for this kind: `edit`, `run/terminal` (delegate these to workers).

### CHILD_ORCHESTRATOR — allowed tools

| Tool | Use |
| --- | --- |
| `search/codebase` | Discovery within assigned scope |
| `read/file` | Read assigned artifacts |
| `agent` | Invoke named workers within assigned scope |

Forbidden for this kind: `edit`, `run/terminal`, cross-scope expansion.

### WORKER — allowed tools

| Tool | Use |
| --- | --- |
| `edit` | Edit files within assigned boundary |
| `read/file` | Read files for context |
| `search/codebase` | Search within scope |
| `run/terminal` | Run validation commands |
| `read/terminalLastCommand` | Check last terminal output |

Forbidden for this kind: `agent` (no subagent invocation without explicit parent authorization in the assignment).

## Tool Reference Prose Syntax

When referencing tools in prose, use `#tool:<tool-name>` syntax.

- Use `#tool:agent` to invoke a sub-agent.
- Use `#tool:edit` to write file changes.
- Use `#tool:search/codebase` to discover workspace artifacts.
- Use `#tool:run/terminal` to run shell commands.

## Optional Frontmatter Fields

```yaml
agents:                 # Sub-agents this agent may invoke (orchestrators only)
  - AppointChildOrchestrator
  - AppointWorker
user-invocable: true    # Show in agent picker (default: true)
disable-model-invocation: false  # Prevent auto-invocation as subagent (default: false)
hooks:                  # Agent-scoped hooks (run only when this agent is active)
  PostToolUse:
    - type: command
      command: "${PLUGIN_ROOT}/scripts/post-tool-use.sh"
handoffs:               # Suggested next steps after agent response
  - label: "Return to Orchestrator"
    agent: AppointOrchestrator
    prompt: "Implementation complete. Please review and finalize."
    send: false
```

## Body Content Structure

```markdown
# Agent Name

## AGENT_KIND
`PARENT_ORCHESTRATOR` | `CHILD_ORCHESTRATOR` | `WORKER`

## 5W1H
| Field | Value |
| --- | --- |
| Who   | ... |
| What  | ... |
| When  | ... |
| Where | ... |
| Why   | ... |
| How   | ... |

## Role
Brief description of the agent's primary function and authority boundary.

## Authority Boundary
- You own: [list of owned responsibilities]
- You delegate: [what you delegate and to whom]
- You forbid from yourself: [what this agent must not do]

## Routing Rule
How decisions are made: explicit routing first, semantic allowance scoped to
authority level.

## Allowed Tools
Least-privilege tool list for this AGENT_KIND.

## Forbidden Tools for This Role
Tools this AGENT_KIND must not use.

## Handoff Contract
Required fields in outgoing assignments and incoming receipts.

## Completion Gate
Conditions that must be true before this agent marks work complete.

## Anti-Drift Gates
Hard rules that prevent scope creep, unverified claims, and missing proof.
```

## Handoff Gate Requirements

Every agent handoff must include all required fields. Orchestrators must reject
receipts that omit these fields.

| Required field | Purpose |
| --- | --- |
| `slice_id` | Maps receipt to the dispatched slice |
| `artifact_kind` | Confirms the kind of artifact touched |
| `changed_files` | Exact list of changed paths |
| `evidence_used` | Sources grounding the changes |
| `assumptions_rejected` | What was explicitly excluded |
| `validation_run` | What checks ran or why they could not |
| `validation_result` | `PASS` or `BLOCK` with reason |
| `blockers` | Unresolved issues |
| `release_decision` | `PASS` or `BLOCK` |
| `fallback_path_if_no_memory` | Fallback audit artifact path when `#memory` is unavailable |

## Memory and Handoff Rule

- Use `#memory` for handoff review where the runtime supports it.
- Do not require or reference `#resolveMemoryFileUri` — this alias is
  unsupported and non-authoritative in this repository.
- If `#memory` is unavailable, write a fallback audit artifact under
  `.runtime/**`, `.requests/**`, or an equivalent path selected before
  execution starts.

## Completion Gate Requirements

An agent must not mark work complete until:

1. All changed files are listed with evidence.
2. Validation has run and the result is recorded (or the blocker for why it
   could not run is recorded).
3. Memory handoff or fallback artifact path is confirmed.
4. No open blockers remain unresolved.
