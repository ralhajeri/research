# Traceability Matrix

Use this matrix with [validation-checklist.md](./validation-checklist.md) and
[final-audit-report.md](./final-audit-report.md) to verify that every RF6 rule
maps to a governed file, deterministic evidence, and a final-audit gate.

## Ownership and Audit Boundary

- Owner: `tests/traceability-matrix.md`
- Input: RF6 request authority, current plugin manifest, current agent files,
  validation checklist, final audit report, and repo-memory inventory.
- Output: One RF6 rule-to-evidence map with no orphan rule.
- Dependency: `plugin.json`, the three agent files,
  `validation-checklist.md`, `final-audit-report.md`,
  `research/.requests/06.md`, and `/memories/repo/`.
- Validator: Traceability Auditor.
- Release gate: RF6 closure stays blocked if any rule below lacks a file owner,
  deterministic evidence path, or final-audit gate.

## Relationship Map

| Node                       | Typed Links                                                                                     |
| -------------------------- | ----------------------------------------------------------------------------------------------- |
| `research/.requests/06.md` | `governs` runtime-root, visibility, tooling, closure, and memory-hygiene rules                  |
| `plugin.json`              | `implements` the runtime-root anchor and `supports` coach/tutor/quizzer discovery               |
| Coach agent                | `implements` the sole user-invocable entry and `depends_on` tutor/quizzer subagent availability |
| Tutor agent                | `implements` the hidden tutoring subagent boundary                                              |
| Quizzer agent              | `implements` the hidden quizzer subagent boundary                                               |
| Validation checklist       | `validates` deterministic RF6 checkpoints                                                       |
| Final audit report         | `blocks_or_passes` RF6 completion based on the governed artifact state                          |
| Repo memory inventory      | `validates` RF6 memory hygiene before release                                                   |

## RF6 Rule-to-Evidence Matrix

| Rule ID | RF6 Rule                                                                    | Owning Files                                                                                                           | Deterministic Evidence                                                                                                                         | Validation Checkpoints                                   | Final Audit Gate                      |
| ------- | --------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------- |
| RF6-01  | `plugin.json` remains the runtime-root anchor                               | `plugin.json`, `research/.requests/06.md`                                                                              | Root `plugin.json` exists and remains the manifest referenced by RF6                                                                           | Official Doc Evidence 1; Runtime Root and Visibility 1-2 | Official docs and runtime-root anchor |
| RF6-02  | Coach is the sole user-invocable agent                                      | `agents/appoint-medical-student-coach.agent.md`, `research/.requests/06.md`                                            | Coach frontmatter contains `user-invocable: true` and no sibling file does                                                                     | Runtime Root and Visibility 3, 6, 8                      | Sole user-facing coach                |
| RF6-03  | Tutor is hidden but callable as a subagent                                  | `agents/appoint-medical-tutor.agent.md`, `agents/appoint-medical-student-coach.agent.md`, `research/.requests/06.md`   | Tutor frontmatter contains `user-invocable: false`; coach `agents` list names tutor; tutor does not set `disable-model-invocation: true`       | Runtime Root and Visibility 4, 6-7                       | Tutor subagent-only                   |
| RF6-04  | Quizzer is hidden but callable as a subagent                                | `agents/appoint-medical-quizzer.agent.md`, `agents/appoint-medical-student-coach.agent.md`, `research/.requests/06.md` | Quizzer frontmatter contains `user-invocable: false`; coach `agents` list names quizzer; quizzer does not set `disable-model-invocation: true` | Runtime Root and Visibility 5-7                          | Quizzer subagent-only                 |
| RF6-05  | Every runtime agent exposes only `todo`, `memory`, and `agent`              | All three agent files, `research/.requests/06.md`                                                                      | Each frontmatter `tools` list exactly matches the RF6 triple and contains no extra entry                                                       | Tool Surface and Collaboration 1-3                       | Exact RF6 tool surface                |
| RF6-06  | No runtime evidence tool or unsupported runtime surface was added           | `plugin.json`, all three agent files, `research/.requests/06.md`                                                       | No hook, MCP, web/search/edit/terminal, or evidence-collector surface added in governed files                                                  | Tool Surface and Collaboration 4-5                       | No widened runtime surface            |
| RF6-07  | Education-only and no-PHI boundaries remain intact                          | All three agent files, `plugin.json` context, `research/.requests/06.md`                                               | Agent bodies still refuse PHI and diagnosis/treatment/prescribing and keep context session-local                                               | Safety and Boundary 1-4                                  | Safety boundary preserved             |
| RF6-08  | Official VS Code docs were revalidated before relying on frontmatter fields | `validation-checklist.md`, `final-audit-report.md`, `research/.requests/06.md`                                         | Validation and final-audit artifacts record the doc revalidation and block unsupported fields                                                  | Official Doc Evidence 1-4                                | Official-doc revalidation             |
| RF6-09  | Request closure occurs only after the governed artifacts satisfy RF6        | `research/.requests/06.md`, `validation-checklist.md`, `final-audit-report.md`                                         | Both `output_intent` cells read `IMPLEMENT (Done)` only after prior gates pass                                                                 | Governed Artifact and Closure 1-4                        | Request closure                       |
| RF6-10  | Repo-memory hygiene is confirmed before release                             | `/memories/repo/`, `validation-checklist.md`, `final-audit-report.md`                                                  | RF6 controlling memory remains active, RF1 remains compatible, and no conflicting RF2-RF5 file remains                                         | Governed Artifact and Closure 5                          | Repo-memory hygiene                   |

## Orphan Review

| Item Type           | Result | Notes                                            |
| ------------------- | ------ | ------------------------------------------------ |
| Rule ownership      | pass   | Each RF6 rule maps to one or more governed files |
| Validation linkage  | pass   | Every RF6 rule points to a checklist checkpoint  |
| Final-audit linkage | pass   | Every RF6 rule points to a final-audit gate      |
| Repo-memory linkage | pass   | Memory hygiene is explicitly attached to RF6-10  |

## Closure Rules

- Any unsupported frontmatter assumption keeps RF6-08 blocked.
- Any visibility or tool-surface mismatch reopens RF6-02 through RF6-06.
- Any missing request-closure or memory-hygiene evidence reopens RF6-09 or
  RF6-10.

## Bounded Confidence Statement

This matrix traces deterministic RF6 artifact ownership and evidence only. It
does not claim mathematical certainty.
