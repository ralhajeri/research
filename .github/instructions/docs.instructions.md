---
name: "Appoint Documentation Writing Guidelines"
description: "Keeps Markdown docs normalized, self-contained, semantically owned, linked, evidence-bounded, Core/Shared-aware, duplication/conflict free, and extendable while preserving structure and validating with deterministic checks and auditor review."
applyTo: "**/*.md"
---

# Appoint Documentation Writing Guidelines

Hard rule: Agents may edit, rename, refactor, or restructure documents only when the change is semantically owned, normalized with related documents, traceable to the controlling request, and validated against the global relation/process/trace/I/O/plugin/tooling contracts. Agents must not add random, duplicated, conflicting, orphaned, or unlinked information. Any new or changed content must identify its owner, source relationship, affected documents, validation path, output boundary, and final auditor gate before completion. Never mention the Request </nn> in the final documents Like `This folder owns the Request 12 shared migration-governance layer.` and all the `Request nn` wording must be found and clened.

## 1. Purpose and Scope

Use these instructions when creating, updating, normalizing, refactoring, or auditing Markdown documentation.

The goal is to preserve important content while keeping every document:

- structurally stable;
- self-contained within its folder;
- semantically owned;
- linked through explicit typed relationships;
- Core/Shared/Local separated;
- evidence-bounded;
- duplication/conflict free;
- validated with deterministic checks where possible;
- subject to final auditor review when implementation output exists.

## 2. Non-Negotiable Rules

- Preserve the existing template, numbering, trace chain, and ownership model unless the task explicitly requires normalization or refactor.
- Keep each folder self-contained.
- Do not reference documents outside the current folder unless the target is in an allowed `_shared/**` folder.
- Use relative links.
- Place every change in the correct logical owner section.
- Do not randomly append content.
- Do not rewrite, shrink, or remove important content unless the task explicitly requires normalization and the replacement preserves the same meaning.
- Do not introduce duplicate concepts already owned elsewhere.
- Do not introduce conflicting terminology, requirements, links, ownership claims, status values, or validation rules.
- Every material node must have typed links to its semantic owner, dependencies, inputs, outputs, validators, and any applicable Core or Shared contract/entity.
- Complete relation mapping before writing.
- Keep claims evidence-bounded.
- Do not make unsupported platform, vendor, tool, legal, security, privacy, architecture, or operational claims.
- Do not claim perfect correctness, guaranteed completeness, mathematical certainty, or false perfection.
- Final auditor validation is required when implementation output exists.

## 3. Semantic Control Model

Every material documentation change must preserve meaning across these layers.

| Layer           | Owns                                                                                                     | Required Control                                                                                                                                                          |
| --------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Terminology     | `controlled_vocabulary`, `glossary_design`, `naming_convention`, `lexicon_design`, `ubiquitous_language` | Use one canonical term per concept. Correct ambiguous or unsupported terms before they govern implementation, acceptance criteria, section placement, or route decisions. |
| Taxonomy        | `category_structure`, `classification_rules`, `parent_child_groups`, `component_types`, `domain_areas`   | Keep labels stable across artifacts, folders, sections, entities, dependencies, slices, file plans, and trace rows.                                                       |
| Ontology        | `entities`, `relationships`, `attributes`, `constraints`, `semantic_rules`                               | Define each entity, its role, its semantic owner, and its constraints before using it in criteria, slices, placement, route decisions, or findings.                       |
| Knowledge Graph | `nodes`, `edges`, `entity_links`, `relationship_mapping`, `context_links`                                | Link every material node with typed relationships to owners, dependencies, inputs, outputs, validators, and applicable Core/Shared contracts or entities.                 |

Required graph behavior:

- Use typed links such as `owns`, `depends_on`, `implements`, `validates`, `extends`, `constrains`, `references`, and `supersedes`.
- Do not allow orphan nodes.
- Every node must link to at least one owner, dependency, validator, or governing contract/entity.
- Record semantic links whenever multiple files or slices must be read together.
- Record which section owns each mutation and which nearby sections are explicitly non-owning.
- Do not let supportive context become hidden execution authority unless deliberately promoted by the user or baseline.

## 4. Core, Shared, and Local Separation of Concerns

Use Core, Shared, and Local boundaries to prevent duplicate ownership and hidden coupling.

| Boundary | Owns                                                                                                            | Must Not Own                                                  |
| -------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| Core     | canonical domain meaning, durable entities, required contracts, invariant rules, architecture-level constraints | local examples, local implementation notes, temporary patches |
| Shared   | reusable templates, schemas, shared examples, shared references, cross-folder support assets                    | canonical entity ownership unless explicitly promoted to Core |
| Local    | folder-specific usage, implementation notes, decisions, validations, examples                                   | duplicated Core or Shared definitions                         |

Rules:

- Do not place local implementation detail in Core.
- Do not place canonical entity or contract definitions only in Local folders when multiple folders depend on them.
- Do not duplicate Core or Shared content locally; link to it with a typed relationship.
- Every cross-folder dependency must identify whether it links to a `core_contract`, `core_entity`, `shared_contract`, `shared_schema`, `shared_template`, or `shared_reference`.
- Keep folder-local links unless linking to `_shared/**`.

## 5. Semantic Ownership and Change Placement

Before editing, identify the semantic owner for the proposed change.

For each material change, preserve or record:

| Field           | Required Meaning                                                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------ |
| Owner           | Folder, file, and section that semantically owns the content.                                                |
| Input           | Source request, requirement, evidence, or dependency driving the change.                                     |
| Output          | Expected document state after the change.                                                                    |
| Dependency      | File, section, schema, diagram, policy, or `_shared/**` reference needed to understand the change.           |
| Validator       | Person, process, audit gate, or deterministic check that verifies the change.                                |
| Non-owner zones | Nearby sections that must not receive the content because they would create drift, duplication, or conflict. |

Placement rules:

- Prefer editing, renaming, remapping, or moving content into the correct owning file.
- Rename or remap files when the current structure no longer matches the semantic model.
- Update links after any rename, remap, or move.
- If structure is wrong, fix the structure instead of adding around the problem.
- Avoid `Patch Addendum` as a permanent pattern.
- Use `Patch Addendum` only as a temporary repair note when immediate structural normalization is unsafe.
- When safe, merge any addendum back into the correct owner section and remove the addendum.
- Mark related docs, schemas, or diagrams as updated, not impacted, or blocked with a reason.

## 6. Contract and Traceability

Every material documentation change must define or preserve:

- **Inputs:** source request, evidence, requirement, dependency, or accepted baseline.
- **Outputs:** resulting document state, section, table, checklist, schema reference, or trace row.
- **Owner:** logical owner folder, file, and section.
- **Dependencies:** upstream and downstream documents, diagrams, schemas, `_shared/**` references, Core contracts/entities, or validators.
- **Validation/audit owner:** reviewer, auditor, deterministic check, or final gate.
- **Pass/block/reopen behavior:** what allows release, what blocks release, and what reopens the change.

Blocked work must route or escalate by relation graph instead of being silently appended, skipped, or guessed.

## 7. Claims, Evidence, and Bounded Confidence

- Treat unsupported entities, relationships, requirements, validation claims, and architecture claims as unverified until validated.
- Mark evidence-gated claims as `evidence-required` until objective evidence is available.
- Preserve bounded confidence statements when the source artifact requires them.
- Do not make unsupported platform, vendor, tool, standard, legal, security, privacy, or operational claims.
- Do not claim perfect correctness, guaranteed completeness, mathematical certainty, or false perfection.
- Keep deterministic claims limited to checks that were actually performed.

## 8. Schema and Deterministic Validation

Where applicable:

- Include schema references.
- Keep required fields, status values, and enum values consistent.
- Validate frontmatter syntax before release.
- Validate heading structure before release.
- Validate relative links before release.
- Validate duplicate or conflicting concepts before release.
- Validate relation mapping before writing or release.
- Validate Core, Shared, and Local ownership boundaries before release.
- Validate that final auditor review is required when implementation output exists.

## 9. Quality Gate Checklist

### Purpose and Audience

- [ ] Document purpose is clear.
- [ ] Target audience is clear.
- [ ] The document explains when to use it.

### Semantic Ownership

- [ ] Content is placed in the correct owning folder, file, and section.
- [ ] No random append is present.
- [ ] No duplicate concept is introduced where the concept is already owned elsewhere.
- [ ] Non-owner zones are respected.
- [ ] If structure is wrong, content is renamed, remapped, or moved instead of added around the problem.

### Self-Contained Folder Rule

- [ ] The folder can be understood by itself.
- [ ] No references point outside the folder unless the target is inside allowed `_shared/**`.
- [ ] Links are relative.
- [ ] Broken or stale links are not present.

### IA Knowledge and Meaning Design

- [ ] Terminology is controlled and consistent.
- [ ] Taxonomy and category labels are stable.
- [ ] Entities and relationships are explicit.
- [ ] Material nodes are linked with typed relationships.
- [ ] Core, Shared, and Local ownership boundaries are explicit.
- [ ] Knowledge graph links are clear across owner, dependency, input, output, validator, and governing contract/entity.

### Contract and Traceability

- [ ] Inputs are defined.
- [ ] Outputs are defined.
- [ ] Owner is defined.
- [ ] Dependencies are defined.
- [ ] Validation/audit owner is defined.
- [ ] Pass, block, and reopen behavior is clear.

### Claims and Evidence

- [ ] No unsupported platform, vendor, tool, standard, legal, security, privacy, architecture, or operational claims are present.
- [ ] Evidence-gated claims are marked as `evidence-required`.
- [ ] No false perfection or mathematical-certainty claim is present.
- [ ] Bounded confidence is preserved where required.

### Schema and Change Safety

- [ ] Schema references are included where applicable.
- [ ] Required fields and status values are consistent.
- [ ] Deterministic checks are listed where possible.
- [ ] Frontmatter is valid YAML.
- [ ] Existing template, numbering, and trace chain are preserved unless explicitly normalized.
- [ ] Extensions are normalized into the correct owner section.
- [ ] `Patch Addendum` is used only as a temporary repair note, not permanent structure.
- [ ] Related docs, schemas, and diagrams are updated or marked not impacted.

### Final Gate

- [ ] Relation mapping was completed before writing.
- [ ] Blocked work routes or escalates by relation graph.
- [ ] Final auditor validation is required when implementation output exists.
- [ ] All applicable quality gates pass or are explicitly blocked with a reason.

## 10. Final Validation Rules

Before final delivery, verify:

- deterministic checks were completed where possible;
- schema validation was completed where applicable;
- relation mapping was completed before writing;
- no unsupported claims remain;
- no false perfection claim remains;
- no external-folder references exist except allowed `_shared/**` references;
- every material node has a typed link to its owner, dependency, validator, or governing Core/Shared contract or entity;
- Core, Shared, and Local concerns are separated without duplicate or conflicting ownership;
- the same concept is not renamed in different sections without a correction record;
- taxonomy labels stay stable across entities, slices, file plans, sections, and trace rows;
- ontology constraints do not conflict with route edges, target paths, target sections, Core/Shared ownership, or evidence posture;
- graph links connect slices, files, target sections, dependencies, validators, governing contracts/entities, and handoff edges without orphaned nodes;
- supportive context remains explicitly supportive unless deliberately promoted by the user or baseline;
- semantic placement and semantic mapping are readable by a downstream implementation agent;
- final auditor validation is required when implementation output exists;
- all applicable quality gates pass or are blocked with a recorded reason.
