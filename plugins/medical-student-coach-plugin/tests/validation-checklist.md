# Validation Checklist

Use this checklist with [traceability-matrix.md](./traceability-matrix.md) and
[final-audit-report.md](./final-audit-report.md) before making any RF6 closure
claim for the medical-student-coach-plugin runtime contract.

## Ownership and Audit Boundary

- Owner: `tests/validation-checklist.md`
- Input: RF6 request authority, current plugin manifest, current coach/tutor/quizzer agent files, traceability matrix, final audit report, and repo-memory inventory.
- Output: Deterministic RF6 checkpoint list for runtime root, visibility, tools, safety, request closure, and repo-memory hygiene.
- Dependency: `plugin.json`, the three agent files, `traceability-matrix.md`, `final-audit-report.md`, `research/.requests/06.md`, and `/memories/repo/`.
- Validator: Validation Auditor.
- Release gate: RF6 may close only when every applicable checkpoint below passes or an explicit blocker is recorded.

## Official Doc Evidence Checkpoints

- [ ] Official VS Code Agent Plugins documentation was revalidated for this pass and still confirms a root `plugin.json` manifest plus documented `skills` and `agents` paths.
- [ ] Official VS Code Custom Agents documentation was revalidated for this pass and still confirms `.agent.md` support for `tools`, `agents`, and `user-invocable`.
- [ ] Official VS Code Custom Agents documentation was revalidated for this pass and still confirms that specifying `agents` requires `agent` to be present in `tools`.
- [ ] No unsupported substitute field such as `infer` was introduced to satisfy RF6.

## Runtime Root and Visibility Checkpoints

- [ ] `plugin.json` remains at the plugin root and remains the runtime-root anchor for this plugin.
- [ ] `plugin.json` was not widened beyond the documented manifest fields already required by this repo.
- [ ] `agents/appoint-medical-student-coach.agent.md` exists and declares `user-invocable: true`.
- [ ] `agents/appoint-medical-tutor.agent.md` exists and declares `user-invocable: false`.
- [ ] `agents/appoint-medical-quizzer.agent.md` exists and declares `user-invocable: false`.
- [ ] The coach frontmatter `agents` list is exactly `AppointMedicalTutor` and `AppointMedicalQuizzer`.
- [ ] Tutor and quizzer remain callable as subagents because neither file sets `disable-model-invocation: true`.
- [ ] No additional user-invocable runtime agent surface was introduced.

## Tool Surface and Collaboration Checkpoints

- [ ] Coach frontmatter `tools` is exactly `todo`, `memory`, and `agent`.
- [ ] Tutor frontmatter `tools` is exactly `todo`, `memory`, and `agent`.
- [ ] Quizzer frontmatter `tools` is exactly `todo`, `memory`, and `agent`.
- [ ] No runtime evidence-collection tool, web/search/edit/terminal tool, hook, or MCP surface was added.
- [ ] Tutor and quizzer do not widen downstream collaboration beyond the minimal RF6 design.

## Safety and Boundary Checkpoints

- [ ] Education-only, no-PHI, and no diagnosis/treatment/prescribing boundaries remain explicit across coach, tutor, and quizzer.
- [ ] Tutor and quizzer remain subagent-only while coach remains the sole learner-facing runtime entry.
- [ ] Current-session weak-area or performance context remains bounded and is not promoted to persistent learner or patient storage.
- [ ] No file claims clinical authority, patient-specific decision support, runtime readiness, or mathematical certainty.

## Governed Artifact and Closure Checkpoints

- [ ] `tests/validation-checklist.md` contains deterministic RF6 checkpoints rather than the older askQuestions-only contract.
- [ ] `tests/traceability-matrix.md` maps every RF6 rule to runtime evidence and final-audit review with no orphan rule.
- [ ] `tests/final-audit-report.md` contains explicit RF6 pass/block gates for docs, runtime root, visibility, tools, request closure, and repo-memory hygiene.
- [ ] `research/.requests/06.md` records `output_intent | IMPLEMENT (Done) |` in both control-record and route-output rows.
- [ ] Repo-memory hygiene is confirmed for release: `/memories/repo/rf6-verified-conventions.md` and `/memories/repo/rf6-plan.md` remain the controlling RF6 memory, `rf1-verified-conventions.md` remains compatible, and no conflicting RF2-RF5 repo-memory file remains active.

## Release Decision Rules

- Pass RF6 validation only when all applicable checkpoints above pass for the governed artifact set.
- Block RF6 closure if official doc support is contradicted, any runtime-root or visibility rule drifts, any tool list widens, request closure is incomplete, or repo-memory hygiene fails.
- Keep this checklist limited to governed artifact conformance; it does not claim live runtime invocation or mathematical certainty.

## Bounded Confidence Statement

This checklist records deterministic RF6 artifact validation only. It does not
claim live runtime execution or mathematical certainty.
