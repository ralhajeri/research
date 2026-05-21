# Traceability Matrix

Use this matrix with [agent-behavior-scenarios.md](./agent-behavior-scenarios.md),
[validation-checklist.md](./validation-checklist.md), and
[final-audit-report.md](./final-audit-report.md) to verify that the
foundation, coach, tutor, and quizzer slices all trace to concrete files,
scenario IDs, acceptance ranges, and closure evidence.

## Ownership and Audit Boundary

- Owner: `tests/traceability-matrix.md`
- Input: Reconciled validation authority, predecessor final audits, current plugin surfaces, current validation artifacts, and cited runtime follow-up evidence when needed.
- Output: One slice-to-evidence map for the full static validation layer.
- Dependency: Current manifest, shared skills, current agent files, scenario suite, validation checklist, final audit report, and cited runtime follow-up evidence when needed.
- Validator: Traceability Auditor.
- Release gate: No broader completion claim may pass if any slice, scenario group, or closure evidence is orphaned.
- Prior artifact audit: This file did not exist in the inherited test surface and was created because cross-slice traceability was missing.

## Relationship Map

| Node             | Typed Links                                                                                                           |
| ---------------- | --------------------------------------------------------------------------------------------------------------------- |
| Foundation slice | `owns` manifest and shared-layer constraints, `validates` global refusal rules                                        |
| Coach slice      | `depends_on` foundation, `implements` study coordination, `validated_by` C scenarios                                  |
| Tutor slice      | `depends_on` foundation and coach continuity, `implements` concept teaching, `validated_by` T scenarios               |
| Quizzer slice    | `depends_on` foundation plus coach and tutor continuity, `implements` assessment behavior, `validated_by` Q scenarios |
| Validation layer | `depends_on` all four slices, `validates` coverage and closure, `blocks` unsupported runtime claims                   |

## Slice-to-Evidence Matrix

| Slice                          | Controlled Capability                                                                                                      | Current Implementation Surfaces                                               | Scenario Coverage                  | Acceptance Range                                                                           | Closure Evidence                                                                                                                                                                                                  | Current Static Status                                                                                       |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ---------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| Foundation                     | Plugin identity, shared safety boundary, shared tutoring method, shared quiz source, and no hook or MCP surface            | `plugin.json`, shared skills, `governance/medical-education-ai-governance.md` | F1-F7                              | Foundation AC1-AC10; validation AC1, AC10, AC15-AC19                                       | Predecessor foundation audit, current manifest, current governance record, current scenario suite                                                                                                                 | Pass with runtime-ready claims blocked                                                                      |
| Local source registration      | Workspace-level registration of the local plugin root for documented source discovery                                      | `.vscode/settings.json`                                                       | Not applicable in this static pass | Validation AC2-AC4, AC20-AC25                                                              | Current workspace settings file plus separate post-registration Copilot-log probe                                                                                                                                 | Configured; no positive live discovery signal observed in the available follow-up evidence                  |
| Coach                          | Study coordination, progress reflection, current-session weak-area handling, and recommendation-only next steps            | `agents/appoint-medical-student-coach.agent.md`                               | C1-C10                             | Coach AC1-AC10; validation AC11, AC15, AC19, AC24                                          | Current coach file, current scenario suite, current checklist, final static audit gate                                                                                                                            | Pass                                                                                                        |
| Tutor                          | Concept teaching, Socratic questioning, misconception correction, uncertainty handling, and recommendation-only next steps | `agents/appoint-medical-tutor.agent.md`                                       | T1-T12                             | Tutor AC1-AC12; validation AC12, AC15, AC19, AC24                                          | Current tutor file, current scenario suite, current checklist, final static audit gate                                                                                                                            | Pass                                                                                                        |
| Quizzer                        | Quiz generation, grading, distractor explanation, current-session weak-area detection, and recommendation-only next steps  | `agents/appoint-medical-quizzer.agent.md`                                     | Q1-Q15                             | Quizzer AC1-AC15; validation AC13, AC15, AC19, AC24                                        | Current quizzer file, current scenario suite, current checklist, final static audit gate                                                                                                                          | Pass                                                                                                        |
| Cross-slice safety and privacy | Education-only posture, PHI refusal, no diagnosis or treatment or prescribing, no clinical decision-support identity       | Governance record plus all three agent files                                  | F3-F4, C5-C6, T9-T10, Q13-Q14      | Foundation AC10; coach AC3-AC4; tutor AC3-AC4; quizzer AC3-AC4; validation AC8, AC15, AC23 | Governance record, scenario suite, checklist, final audit blocker rules                                                                                                                                           | Pass                                                                                                        |
| Cross-slice role boundaries    | Coach, tutor, and quizzer remain separate and non-invoking                                                                 | Governance record plus all three agent files                                  | C2-C3, C8-C9, T7-T8, T12, Q11-Q12  | Coach AC6-AC10; tutor AC7-AC12; quizzer AC10-AC15; validation AC7, AC15, AC24              | Governance record, scenario suite, checklist, final audit blocker rules                                                                                                                                           | Pass                                                                                                        |
| Records and session locality   | Weak-area and performance summaries stay current-session only                                                              | Coach and quizzer files, plus tutor non-persistence rules                     | C4, C10, Q11-Q12                   | Coach AC5, AC9; tutor AC10; quizzer AC9, AC13; validation AC15, AC24                       | Current agent files, checklist, final static audit gate                                                                                                                                                           | Pass                                                                                                        |
| Runtime evidence gate          | Runtime-ready, live-load, live-discovery, and actual-invocation claims require separate proof                              | Governance record, validation checklist, final audit report                   | Not applicable in this static pass | Validation AC2-AC4, AC20-AC25                                                              | Official-doc review dated 2026-05-21, local VS Code Insiders baseline capture, workspace local-plugin registration, post-registration Copilot-log probe, final audit report, and explicit evidence-required notes | Platform-doc surface proof, environment baseline, and local registration captured; live proof still blocked |

## Orphan Review

| Item Type         | Result            | Notes                                                                                                                                                                                                                                                  |
| ----------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Slice ownership   | pass              | Foundation, coach, tutor, quizzer, and validation surfaces each have a named owner                                                                                                                                                                     |
| Scenario coverage | pass              | Every required scenario group is represented in one scenario suite                                                                                                                                                                                     |
| Closure evidence  | pass              | Each slice points to current file evidence plus final audit gating                                                                                                                                                                                     |
| Runtime proof     | partially bounded | Current official-doc surface proof, local environment baseline, and separate workspace-local registration evidence are present, but no positive discovery/load signal and no actual invocation proof were observed in the available follow-up evidence |

## Closure Rules

- Any missing file, scenario group, or acceptance range keeps the final audit
  blocked.
- Any unsupported runtime-ready, live-load, live-discovery, or actual
  invocation claim keeps the final audit blocked.
- Any safety, privacy, or role-boundary drift reopens the affected slice and
  its final-audit row.

## Bounded Confidence Statement

This matrix traces the current static validation layer only. It does not prove
runtime readiness, live plugin loading, actual sibling-agent invocation, or
mathematical certainty.
