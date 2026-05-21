# Validation Checklist

Use this checklist with [agent-behavior-scenarios.md](./agent-behavior-scenarios.md),
[traceability-matrix.md](./traceability-matrix.md), and
[final-audit-report.md](./final-audit-report.md) before making any closure
claim for the medical-student-coach-plugin validation layer.

## Ownership and Audit Boundary

| Field                | Value                                                                                                                                                                                                      |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Owner                | `tests/validation-checklist.md`                                                                                                                                                                            |
| Input                | Reconciled validation authority, predecessor audits, current plugin surfaces, the audited inherited foundation-only checklist, and separately cited workspace-local runtime-follow-up evidence when needed |
| Output               | One deterministic checklist for the full foundation, coach, tutor, quizzer, traceability, and final-audit layer                                                                                            |
| Dependency           | Current manifest, shared skills, current agent files, scenario suite, traceability matrix, final audit report, and separately cited workspace-local runtime-follow-up evidence when needed                 |
| Validator            | Validation Auditor                                                                                                                                                                                         |
| Release gate         | Checklist completion may support a static audit, but runtime-ready, live-load, and actual-invocation claims stay blocked without separate evidence                                                         |
| Prior artifact audit | The inherited checklist was reviewed first and replaced because it covered only the foundation slice and did not govern cross-agent validation, traceability, or final-audit closure                       |

## Deterministic Structure Checks

- [ ] `plugin.json` exists at the plugin root and parses as JSON.
- [ ] `plugin.json` keeps only the currently proven static fields unless a
      separately accepted authority expands the schema.
- [ ] `plugin.json` points `skills` to `skills/` and `agents` to `agents/`
      for the current documented plugin surface.
- [ ] If separate local source registration evidence is used,
      `.vscode/settings.json` exists and maps `chat.pluginLocations` to the
      local plugin root.
- [ ] The plugin root contains exactly these agent surfaces for the current
      static slice: coach, tutor, and quizzer.
- [ ] Coach, tutor, and quizzer frontmatter `tools` arrays are limited to
      `vscode/askQuestions` for bounded clarification only, with no broader
      built-in tool surface authorized.
- [ ] `governance/medical-education-ai-governance.md` exists.
- [ ] `tests/agent-behavior-scenarios.md` exists.
- [ ] `tests/validation-checklist.md` exists.
- [ ] `tests/traceability-matrix.md` exists.
- [ ] `tests/final-audit-report.md` exists.
- [ ] No hook configuration, MCP server surface, or related runtime pack is
      present under the plugin root.
- [ ] No unauthorized extra agent, score-storage, analytics, or runtime-only
      surface has been introduced.

## Safety and Privacy Checks

- [ ] Education-only scope is explicit across governance and all agent files.
- [ ] PHI refusal is explicit across foundation, coach, tutor, and quizzer
      scenario coverage.
- [ ] Real-patient diagnosis, treatment, prescribing, urgent-care guidance,
      and clinical decision-support identity are prohibited.
- [ ] Fictional or de-identified teaching examples remain the only allowed
      case format.
- [ ] No patient data store, persistent weak-area memory, persistent score
      storage, or learner analytics surface is authorized.
- [ ] No file overclaims clinical authority, runtime readiness, or production
      readiness.

## Role Boundary and Handoff Checks

- [ ] The coach remains limited to study coordination, progress reflection,
      and current-session weak-area handling.
- [ ] The tutor remains limited to concept teaching, guided questions, and
      misconception correction.
- [ ] The quizzer remains limited to quiz generation, grading, explanation,
      and current-session weak-area detection.
- [ ] Cross-agent handoffs remain recommendation-only or conceptual-routing
      only.
- [ ] No direct sibling-agent invocation is described or implied.
- [ ] Shared governance, tutoring method, and quiz behavior remain inherited
      shared layers rather than duplicated local rules.

## Scenario Coverage Checks

- [ ] Foundation scenarios F1-F7 are present.
- [ ] Coach scenarios C1-C10 are present.
- [ ] Tutor scenarios T1-T12 are present.
- [ ] Quizzer scenarios Q1-Q15 are present.
- [ ] Scenario descriptions preserve PHI refusal, safety refusal, role
      boundaries, and session-local records posture.
- [ ] Scenario descriptions do not claim live execution, live discovery, or
      runtime proof.

## Evidence, Traceability, and Closure Checks

- [ ] Current official platform-doc evidence covers the manifest and
      `.agent.md` surfaces used by this repo before any runtime-facing claim.
- [ ] The target environment baseline is recorded separately before any
      runtime-ready, live-load, or live-discovery claim.
- [ ] Any local source registration evidence is kept separate from positive
      live discovery or live load proof.
- [ ] The traceability matrix maps the foundation, coach, tutor, and quizzer
      slices to files, scenarios, acceptance criteria, and closure evidence.
- [ ] The traceability matrix has no orphan slice, scenario group, or audit
      dependency.
- [ ] The final audit report includes explicit pass or fail fields.
- [ ] The final audit report blocks completion if platform-doc validation is
      missing.
- [ ] The final audit report blocks completion if environment proof is
      missing.
- [ ] The final audit report blocks completion if any safety, PHI, or
      role-boundary check fails.
- [ ] The final audit report blocks runtime-ready, live-load, and
      actual-invocation claims without separate evidence.
- [ ] Any cited request-side testing receipt stays explicitly bounded to the
      sampled current-session interactions and safety cases it actually
      records.
- [ ] Any cited request-side testing receipt is described as separate
      follow-up evidence only and is not promoted to plugin discovery,
      plugin load, runtime-ready, generalized multi-agent execution, or
      direct sibling-invocation proof.
- [ ] No validation artifact claims live discovery or actual invocation in
      this static pass.

## Release Decision Rules

- Pass static validation only when all applicable structure, safety, role,
  scenario, traceability, and final-audit checks pass for the current static
  slice.
- Block any broader completion claim if platform-doc evidence, environment
  proof, safety coverage, PHI refusal, role-boundary integrity, or traceable
  closure evidence is missing.
- Keep runtime-ready, live-load, and actual-invocation claims blocked until a
  separate evidence-backed pass proves them.

## Bounded Confidence Statement

This checklist governs deterministic static validation only. It does not prove
runtime readiness, live plugin loading, actual agent invocation, or
mathematical certainty.
