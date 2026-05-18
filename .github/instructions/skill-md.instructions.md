---
name: "Appoint Skill MD Instructions"
description:
  "Instructions for authoring SKILL.md files per the agentskills.io
  specification in Appoint AI plugins."
applyTo: "**/SKILL.md"
---

<!-- @format -->

# Skill File Instructions (`SKILL.md`) — agentskills.io Specification

These instructions apply to all `SKILL.md` files in Appoint AI plugin
skills directories. Follow the
[agentskills.io specification](https://agentskills.io/specification)
strictly.

## Naming Convention

- Skill `name` in frontmatter: `appoint-<name>` (kebab-case)
- Skill directory name MUST exactly match the `name` field
- Example: `skills/appoint-core/SKILL.md` → `name: appoint-core`

> ⚠️ If the directory name does not match `name`, the skill **silently
> fails to load**. No namespace prefixes (e.g., `myorg/skill-name`) — these
> also cause silent failures.

## Required SKILL.md Structure

```markdown
---
name: appoint-<skill-name> # kebab-case, MUST match parent directory name
description: >
  What this skill does AND when to use it. Be explicit about both
  capabilities and trigger conditions. This description drives auto-loading
  by Copilot. Maximum 1024 characters.
argument-hint: "[optional] [arguments]" # optional: shown in slash command hint
user-invocable: true # optional: appears in / menu (default: true)
disable-model-invocation: false # optional: prevents auto-load (default: false)
---

# Skill Name

Brief one-paragraph description.

## When to use this skill

Explicit list of conditions that should trigger this skill.

## Directory structure

\`\`\` skills/ └── appoint-<skill-name>/ ├── SKILL.md ← this file ├──
scripts/ │ └── execute.sh ← [execute script](./scripts/execute.sh) ├──
examples/ │ └── example.md └── references/ └── reference.md \`\`\`

## Instructions

Step-by-step procedures the agent should follow.

## Examples

Usage examples with expected inputs and outputs.
```

## Frontmatter Field Reference

| Field                      | Required | Description                                                         |
| -------------------------- | -------- | ------------------------------------------------------------------- |
| `name`                     | ✅       | Kebab-case identifier, must match directory name, starts `appoint-` |
| `description`              | ✅       | What skill does AND when to use it (max 1024 chars)                 |
| `argument-hint`            | ❌       | Hint text shown in slash command                                    |
| `user-invocable`           | ❌       | Show in `/` menu (default: `true`)                                  |
| `disable-model-invocation` | ❌       | Require manual `/` invocation only (default: `false`)               |
| `context`                  | ❌       | `fork` to run in isolated subagent context (experimental)           |

## Referencing Supporting Files

Always reference scripts and resources in the body using relative Markdown
links:

```markdown
Run the [execute script](./scripts/execute.sh) to perform this operation.
See [example usage](./examples/example.md) for reference.
```

> Unreferenced files in the skill directory are **not loaded** by the
> agent.

## Multi-Workflow Skill Structure

A skill may serve a single use-case (single-workflow) or multiple related
use-cases (multi-workflow). Choose based on the number of distinct
workflows/intents the skill covers.

### When to use a single-workflow skill

Use a plain flat skill directory when the skill has **exactly one workflow
or intent**:

```text
skills/
└── appoint-<skill-name>/
    ├── SKILL.md              ← routing: always leads to the same procedure
    ├── scripts/
    │   └── execute.sh
    ├── examples/
    └── references/
```

### When to use a multi-workflow skill

Use the `workflows/` sub-directory when the skill covers **two or more
distinct workflows, diagram intents, or use-cases** that each require
separate instructions and acceptance criteria.

> **Rule**: If a user's request can lead to two different procedures with
> different shapes, layouts, or validation rules, it belongs in separate
> workflow sub-directories.

#### Directory structure (multi-workflow)

```text
skills/
└── appoint-<skill-name>/
    ├── SKILL.md                       ← Required: routing hub + intent classification
    ├── workflows/                     ← Required (multi-workflow): one sub-dir per workflow
    │   ├── <workflow-a>/
    │   │   └── workflow.md            ← Acceptance criteria + step-by-step instructions
    │   ├── <workflow-b>/
    │   │   └── workflow.md
    │   └── <workflow-c>/
    │       ├── workflow.md
    │       └── template.ext           ← Optional: starter template for this workflow
    ├── references/                    ← Optional: shared reference docs across workflows
    │   └── concept-guide.md
    ├── scripts/                       ← Optional: shared scripts
    │   └── validate.sh
    └── examples/                      ← Optional: shared examples
```

#### SKILL.md routing requirements (multi-workflow)

The `SKILL.md` body **must** contain a **Routing** section that:

1. Lists every workflow by name with its trigger conditions
2. Contains an explicit decision table or branching list
3. Links to each `workflows/<name>/workflow.md` file with a relative
   Markdown link
4. Defines what constitutes an unrecognized intent (fallback behavior)

```markdown
## Routing

Determine which workflow to follow based on the user's intent:

| Intent / Trigger      | Workflow                                         |
| --------------------- | ------------------------------------------------ |
| Condition A (example) | [workflow-a](./workflows/workflow-a/workflow.md) |
| Condition B (example) | [workflow-b](./workflows/workflow-b/workflow.md) |
| Condition C (example) | [workflow-c](./workflows/workflow-c/workflow.md) |
| Unrecognized intent   | Ask the user to clarify before proceeding        |
```

#### workflow.md requirements

Each `workflows/<name>/workflow.md` file must contain these sections in
this order:

1. **Runtime 6W** — who, what, when, where, why, and how the workflow
   adapts at runtime
2. **When to Use** — exact trigger conditions that route here (mirrors the
   SKILL.md routing table)
3. **Input Contract** — typed table of every field the workflow needs
   before it can start
4. **Output Contract** — typed table of every artifact/value the workflow
   produces on completion
5. **Evidence Register** — material claims, sources, status, and
   implication
6. **Falsify Register** — per-intent conditions that would prove the route
   or output invalid
7. **Edge-Case Matrix** — edge cases with detection signal, handling, and
   release effect
8. **Validation Loop** — narrow checks, proof, fallback, and release
   decision
9. **Runtime Expansion Rule** — when to split, add workflows, scripts,
   references, or contracts
10. **Standalone Artifact Rule** — how the workflow output remains complete
    without upstream context
11. **Anti-Leakage Rule** — creator/path/provenance and unresolved
    placeholder leakage to block
12. **Step-by-Step Instructions** — numbered procedure
13. **Acceptance Criteria** — explicit checklist; each item is binary
    (pass/fail)
14. **Edge Cases** — known edge cases and how to handle them

##### Input Contract format

Use a table with columns **Field | Type | Required | Description**:

```markdown
## Input Contract

Collect all of the following before starting. Do not proceed until every
required field is confirmed.

| Field     | Type           | Required | Description                      |
| --------- | -------------- | -------- | -------------------------------- |
| `field-a` | `list<string>` | ✅       | Description of field A           |
| `field-b` | `string`       | ❌       | Optional: description of field B |
```

- Mark mandatory fields with ✅ and optional fields with ❌
- Use typed notation (`string`, `list<T>`, `list<{key, key}>`, `boolean`,
  `file`, `checklist`)
- Every field that the agent must collect from the user or context must
  appear here

##### Output Contract format

Use a table with columns **Field | Type | Description**:

```markdown
## Output Contract

A completed <workflow-name> workflow produces the following artifacts.

| Field                  | Type               | Description                                |
| ---------------------- | ------------------ | ------------------------------------------ |
| `diagram-file`         | `file` (`.drawio`) | Validated file saved under `plans/drawio/` |
| `some-documented-list` | `list<{key, key}>` | Description of the list as produced        |
| `acceptance-checklist` | `checklist`        | All Acceptance Criteria marked pass/fail   |
```

- Every artifact that is consumed by a subsequent step, workflow, or the
  orchestrator must appear
- The `acceptance-checklist` output is **always required** in every
  workflow

##### Workflow transition rule

When the agent re-routes from one workflow to another (e.g., the user
discovers midway that the diagram type was wrong), the agent **must**:

1. Record the Output Contract fields that were already produced by the
   exited workflow
2. Compare them against the Input Contract of the target workflow
3. Collect only the fields that are missing — do not re-collect fields
   already satisfied
4. State the transition explicitly: _"Transitioning from \<old-workflow\>
   to \<new-workflow\>. Carrying over: [field list]. Still needed: [field
   list]."_

##### Complete workflow.md template

```markdown
# Workflow: <workflow-name>

## Runtime 6W

| Field | Value |
| ----- | ----- |
| Who   | ...   |
| What  | ...   |
| When  | ...   |
| Where | ...   |
| Why   | ...   |
| How   | ...   |

## When to Use

Use this workflow when: <exact trigger conditions>.

## Input Contract

Collect all of the following before starting. Do not proceed until every
required field is confirmed.

| Field     | Type           | Required | Description |
| --------- | -------------- | -------- | ----------- |
| `field-a` | `list<string>` | ✅       | ...         |
| `field-b` | `string`       | ❌       | ...         |

## Output Contract

A completed <workflow-name> workflow produces the following artifacts.

| Field                  | Type               | Description                                |
| ---------------------- | ------------------ | ------------------------------------------ |
| `diagram-file`         | `file` (`.drawio`) | Validated file saved under `plans/drawio/` |
| `acceptance-checklist` | `checklist`        | All Acceptance Criteria marked pass/fail   |

## Evidence Register

| Claim | Evidence source | Status | Implication |
| ----- | --------------- | ------ | ----------- |
| ...   | ...             | ...    | ...         |

## Falsify Register

| Condition | Check | Action |
| --------- | ----- | ------ |
| ...       | ...   | ...    |

## Edge-Case Matrix

| Situation | Signal | Handling | Release effect |
| --------- | ------ | -------- | -------------- |
| ...       | ...    | ...      | ...            |

## Validation Loop

| Check | Proof | Fallback | Release decision |
| ----- | ----- | -------- | ---------------- |
| ...   | ...   | ...      | ...              |

## Runtime Expansion Rule

...

## Standalone Artifact Rule

...

## Anti-Leakage Rule

...

## Step-by-Step Instructions

1. Step one
2. Step two
3. Step three

## Acceptance Criteria

Before this workflow output is considered complete:

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Edge Cases

| Situation   | Handling                         |
| ----------- | -------------------------------- |
| Edge case A | Do X instead of Y                |
| Edge case B | Abort and report to orchestrator |
```

> **Important**: `workflow.md` files are **loaded on demand** (three-level
> loading: discovery → routing in SKILL.md → workflow detail). Keep each
> `workflow.md` self-contained.
>
> **Contract enforcement**: the Input Contract and Output Contract are the
> handshake between the routing layer (SKILL.md) and the execution layer
> (workflow.md). They must be kept in sync whenever the workflow's steps or
> acceptance criteria change.

Copilot loads skills progressively:

1. **Discovery**: reads `name` + `description` from frontmatter
2. **Instructions**: loads `SKILL.md` body when relevant
3. **Resources**: loads referenced files on demand

This means you can have many skills installed without context overhead.

## Auto-load vs Manual-invoke

| Setting                          | Slash command | Auto-load | Use Case                    |
| -------------------------------- | ------------- | --------- | --------------------------- |
| Default (both omitted)           | ✅            | ✅        | General-purpose skills      |
| `user-invocable: false`          | ❌            | ✅        | Background knowledge skills |
| `disable-model-invocation: true` | ✅            | ❌        | On-demand only skills       |
| Both `false`/`true`              | ❌            | ❌        | Disabled (not recommended)  |

## Skill Link Boundaries

Every skill must be operationally self-contained. These rules apply to all
skills in all Appoint AI repositories.

### Allowed links

| Link kind | Allowed target |
| --- | --- |
| Same-page anchor | Any heading within the same file |
| In-skill internal link | Any file under the same skill root |
| Same-skill `_shared/**` | Shared contracts, checklists, or validation files under the same skill's `_shared/` directory |
| Official informational URL | An official vendor URL clearly labeled as informational and not required for operation |

### Forbidden links for operational use

| Forbidden pattern | Reason |
| --- | --- |
| Link to another skill root | Cross-skill operational dependency breaks skill isolation |
| Link to a repo-level document as runtime dependency | `docs/**` and other repo files may not be present or stable in downstream plugin contexts |
| Cross-skill shared contract | No two skills may share one operational contract file |
| External non-official URL as authoritative source | Only official vendor documentation may support platform claims |

### Backlink requirements

- Every workflow `workflow.md` must back-link to its own `SKILL.md` hub.
- Every child document under a workflow must back-link to its parent
  workflow hub and to the root `SKILL.md`.
- Every file in a skill's `_shared/**` must back-link to the root
  `SKILL.md`.
- Backlinks must be explicit Markdown links, not implied by prose.

### Anchor resolution

- Every deep-link anchor used in prose or a link target must resolve to an
  actual heading in the target file.
- Do not create placeholder anchors or assume a heading will be added later.

## Template Storage Rules

These rules apply to all templates used in skills and skill workflows.

### One template equals one file

- Each distinct emitted artifact kind requires its own template file.
- Do not combine multiple artifact kinds into one template file.
- Do not create both a `template/` and a `templates/` directory under the
  same workflow; use only `templates/` (plural).

### Template naming

- Template files must be named clearly for their artifact kind.
- Example: `parent-orchestrator.agent.template.md`, `workflow.template.md`,
  `SKILL.template.md`.

### Placeholder rules

- Templates may use fill tokens (e.g., `{{SKILL_NAME}}`,
  `<plugin-name>`) while inside the builder or generator.
- Emitted artifacts must not contain unresolved fill tokens after generation.
- Scan emitted artifacts for unresolved placeholders before release.

### Anti-leakage rule for templates

Before releasing an emitted artifact from a template:

- Remove builder names or paths used as provenance or runtime dependency.
- Remove seed plugin slugs or example-only paths.
- Remove unresolved fill tokens.
- Remove empty sections that claim validation or evidence without proof.
- Remove static template text not adapted to the target intent.

### Standalone output rule

- A template must include enough content that a downstream skill or agent
  can be generated without requiring access to the template file at runtime.
- If a generated skill references external content, that reference must be
  labeled informational-only and not required for operation.
- Generated artifacts must not have a runtime dependency on `docs/**` or
  upstream builder files.
