# Medical Education AI Governance

This document owns the plugin-wide static governance overlay for the
medical-student-coach-plugin validation layer. It governs the shared
foundation, coach, tutor, quizzer, and validation artifacts as one
education-only system. It does not prove live plugin discovery, live agent
availability, direct sibling-agent invocation, or runtime-ready support.

## Ownership and Audit Boundary

- Owner: `governance/medical-education-ai-governance.md`
- Input: Reconciled RF5 authority, predecessor final audits, current plugin surfaces, the audited inherited governance record, and cited runtime follow-up evidence when needed.
- Output: One plugin-wide governance record for the foundation, coach, tutor, quizzer, traceability, and final static audit layer.
- Dependency: Shared governance skill, tutoring method skill, quiz engine skill, coach agent, tutor agent, quizzer agent, tests-folder validation artifacts, and cited runtime follow-up evidence when needed.
- Validator: Traceability Auditor and Validation Auditor.
- Release gate: Static governance coverage may pass, but runtime-ready, live-load, and actual-invocation claims remain blocked without separate platform and environment evidence.
- Prior artifact audit: The inherited foundation-only governance file was reviewed first and fully replaced because it did not govern coach, tutor, quizzer, traceability, or final-audit behavior.

## Governance Relationship Map

| Node                  | Node Type          | Typed Links                                                                                                                                                                         |
| --------------------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Shared foundation     | shared_layer       | `owns` plugin identity and shared safety, `constrains` every agent surface, `depends_on` static manifest and skill inventory                                                        |
| Coach role            | agent_surface      | `depends_on` shared foundation, `implements` study coordination, `constrains` tutor or quizzer transitions to recommendation-only language                                          |
| Tutor role            | agent_surface      | `depends_on` shared foundation and coach continuity, `implements` concept teaching, `constrains` quiz and planning transitions to recommendation-only language                      |
| Quizzer role          | agent_surface      | `depends_on` shared foundation plus coach and tutor continuity, `implements` assessment behavior, `constrains` planning and remediation transitions to recommendation-only language |
| Validation suite      | validation_surface | `depends_on` foundation plus coach plus tutor plus quizzer, `validates` governance coverage, scenarios, traceability, and final audit status                                        |
| Runtime evidence gate | evidence_gate      | `blocks` runtime-ready, live-load, and actual-invocation claims until separate official-doc and environment proof exists                                                            |

## Current Official-Doc and Environment Baseline

- Official VS Code Copilot customization docs were rechecked on 2026-05-21.
  They document agent plugins in preview with a root `plugin.json` manifest,
  documented `skills` and `agents` paths, and custom agents defined in
  `.agent.md` files.
- The same docs also document local plugin registration through the
  `chat.pluginLocations` setting. Separate workspace-local follow-up evidence
  in `.vscode/settings.json` points that setting at the local plugin root.
- The same current docs describe handoffs as guided, user-mediated next steps.
  This repo does not implement those handoffs in the current agent files, so
  direct sibling-agent invocation remains blocked by local contract.
- The local target-environment baseline available to this validation layer is
  VS Code Insiders `1.121.0-insider`. That baseline helps scope runtime
  evidence, but it is not live-load, live-discovery, or actual-invocation
  proof.
- Separate post-registration log-probe evidence found no positive GitHub
  Copilot Chat signal that this local plugin was discovered or loaded in the
  active session, so live discovery remains unproven.

## Inherited Non-Negotiable Rules

- Education-only behavior only.
- No PHI, identifiable patient content, or hidden records collection.
- No diagnosis, treatment, prescribing, urgent-care guidance, or clinical
  decision-support identity.
- No hooks, no MCP servers, and no external runtime side channels.
- No persistent patient memory, persistent weak-area memory, score database,
  leaderboard, or analytics surface.
- No unauthorized direct sibling-agent invocation. Coach, tutor, and quizzer
  transitions are recommendation-only or conceptual-routing only.
- No runtime-ready, live-load, production-readiness, or actual-invocation
  claim without separate platform and environment evidence.

## Cross-Surface Role Boundary

| Surface              | Owns                                                                                                      | Must Not Own                                                                                 | Handoff Posture                                               |
| -------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Shared foundation    | Plugin identity, shared safety terms, shared tutoring method, shared quiz behavior                        | Agent-specific persona ownership, runtime claims, live invocation semantics                  | Governs every other surface                                   |
| Coach                | Study planning, progress reflection, current-session weak-area handling, next-step recommendation         | Full tutoring, full quiz delivery, persistent memory, patient-care advice                    | Recommendation-only or conceptual routing to tutor or quizzer |
| Tutor                | Concept explanation, Socratic questioning, misconception correction, fictional teaching examples          | Study-plan ownership, full quiz delivery, patient-care advice, persistent memory             | Recommendation-only or conceptual routing to coach or quizzer |
| Quizzer              | Quiz generation, grading, distractor explanation, current-session weak-area detection, follow-up practice | Study-plan ownership, full tutoring ownership, persistent score storage, patient-care advice | Recommendation-only or conceptual routing to coach or tutor   |
| Validation artifacts | Governance, scenario coverage, deterministic checks, traceability, final static audit                     | Persona behavior changes, manifest mutation, hooks, MCP servers, runtime proof claims        | Audit-only surface                                            |

## Fourteen-Domain Governance Overlay

| Governance Domain                                      | Control Effect                                                                                         | Static Validation Signal                                           | Block When                                                                |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------------- |
| Data Knowledge and Meaning Design                      | Keep one controlled vocabulary for education-only, fictional case, quiz, weak area, and PHI boundaries | Terms stay stable across governance, agents, and tests             | Clinical-decision-support or patient-care terms replace educational terms |
| Semantic Data Architecture and Governance              | Preserve the ontology of shared layer, coach, tutor, quizzer, and validation surfaces                  | Role ownership and typed dependencies are explicit                 | Hidden role overlap or orphan validation nodes appear                     |
| Information Architecture and Governance                | Keep manifest, agents, governance, and tests in distinct owned locations                               | Approved paths stay stable and readable                            | Validation content leaks into agent personas or unrelated folders         |
| Enterprise Architecture                                | Preserve the shared-layer-plus-three-agent design                                                      | Shared foundation remains the common constraint surface            | Safety or quiz rules are duplicated with conflicting meanings             |
| Secure SDLC and Secure Software Development Governance | Keep the writable surface bounded and the audit deterministic                                          | Approved files only, with explicit pass or block rules             | Unbounded edits or unverified release claims appear                       |
| Software Architecture Governance                       | Preserve manifest, skill, agent, governance, and test boundaries                                       | No validation logic is moved into persona files                    | Agent files or manifest are reopened by validation content                |
| System Architecture and Systems Engineering Governance | Preserve the coach-tutor-quizzer relationship graph                                                    | Recommendation-only transitions are explicit                       | Direct sibling-agent invocation is introduced                             |
| Privacy Engineering and Privacy Risk Governance        | Refuse PHI and block identifiable patient content                                                      | PHI refusal appears in shared, coach, tutor, and quizzer scenarios | Any surface accepts or stores identifiers                                 |
| Cybersecurity Risk Management Governance               | Keep hooks, MCP servers, and external runtime surfaces out of scope                                    | No hook or MCP surface is documented as allowed                    | Hook, MCP, or external runtime semantics appear                           |
| AI Risk Management and AI Governance                   | Keep the system educational, bounded, and uncertainty-aware                                            | Safety refusals and no-runtime-overclaim rules stay explicit       | Diagnosis, treatment, prescribing, or false authority is introduced       |
| Records and Information Management Governance          | Keep weak areas and performance summaries session-local only                                           | No persistent records or score stores are authorized               | Persistent memory or score retention is claimed                           |
| Configuration, Change, and Release Governance          | Preserve the predecessor order and the static-only release posture                                     | Foundation, coach, tutor, and quizzer dependencies remain intact   | Later slices contradict earlier audited constraints                       |
| Requirements and Traceability Governance               | Map slices to scenarios, accepted criteria, and closure evidence                                       | Traceability matrix has no orphan slice                            | Scenario, file, or evidence coverage is missing                           |
| Validation, Verification, and Audit Governance         | Require governance coverage, scenario coverage, deterministic checks, and final audit gating           | Checklist, matrix, and audit report all exist                      | Final completion is claimed without evidence or audit status              |

## Evidence and Release Gates

| Gate                        | Static Result                | Allowed Claim                                                               | Block Condition                                                               |
| --------------------------- | ---------------------------- | --------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Artifact inventory          | pass                         | The validation layer exists and governs the approved surfaces               | Any approved file is missing                                                  |
| Safety and privacy boundary | pass                         | Education-only and PHI refusal rules are explicit across all slices         | Any artifact permits patient-care behavior or identifiable data               |
| Role-boundary integrity     | pass                         | Coach, tutor, and quizzer remain separate, recommendation-only neighbors    | Any artifact implies direct sibling invocation or role drift                  |
| Platform-doc surface proof  | pass                         | Current official docs cover the manifest and `.agent.md` surfaces used here | Current official-doc evidence for the current surfaces is absent              |
| Target environment baseline | pass                         | The local host baseline is known and can scope a runtime-only follow-up     | Any runtime-ready or live-discovery claim is made from version evidence alone |
| Live behavior proof         | not claimed in this document | No actual invocation, live-load, or discovery claim is allowed              | Any artifact implies live execution proof without evidence                    |

## Pass and Block Conditions

- Pass static governance only when all approved validation artifacts exist,
  the inherited safety constraints remain explicit, role boundaries remain
  intact, and the final audit keeps runtime-ready claims blocked without
  separate evidence.
- Block if any artifact accepts PHI, allows diagnosis or treatment or
  prescribing, introduces hooks or MCP servers, implies direct sibling-agent
  invocation, or claims runtime readiness without platform and environment
  proof.

## Bounded Confidence Statement

This governance record is bounded to the static validation layer and current
repo surfaces reviewed for this pass. It may cite separate workspace-local
runtime-follow-up evidence, but that evidence does not change the five-file
authoring boundary or establish runtime readiness, live discovery, live
plugin loading, actual agent invocation, or mathematical certainty.
