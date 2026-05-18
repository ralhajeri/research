---
name: "Plans Draw.io Workflow Instructions"
description:
  "Use when creating, editing, reviewing, or validating draw.io workflow
  files under plans/drawio/, especially actor lanes, handoffs, relation
  validation, connector routing, geometry checks, and semantic rendering
  validation."
applyTo: "plans/drawio/**"
---

<!-- @format -->

# Draw.io Workflow for `plans/drawio/`

## Purpose

Every Appoint AI plugin build **must begin** with a process diagram before
any code is written. The canonical diagram file lives under
`plans/drawio/`. These instructions govern the creation, editing, and
semantic validation of **all** draw.io diagrams in this directory, across
all supported diagram types.

## Scope

- Apply these instructions to all files under `plans/drawio/`.
- Use this workflow for new diagrams, edits to existing diagrams, and
  semantic validation passes.
- Do not treat copies of a diagram outside this directory as the primary
  editing target.
- Every diagram in this directory must be a standalone proof artifact: it
  must contain the diagram type, relevant actors/entities/states/messages,
  labels, and validation-relevant semantics in the XML itself.
- Diagram XML and labels must not contain unresolved fill tokens, seed-only
  paths, or creator skill/provenance text as runtime dependency.
- When creating a new diagram, **first complete §0** to select the correct
  diagram type. Each diagram type has its own shape palette, layout rules,
  and completion checklist.

---

## §0. Choose Your Diagram Type First

Before touching any XML, select the diagram type that best represents the
intent. Using the wrong type forces unnatural representations and produces
misleading artefacts.

### §0a. Decision Table

| Question                                                                                                             | Answer → Diagram Type          |
| -------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| Does the diagram show a **multi-step process across multiple actors**?                                               | → **Swimlane Process** (§4–9)  |
| Does the diagram show **who calls whom and in what order** (timing)?                                                 | → **Sequence Diagram** (§12)   |
| Does the diagram show **states and transitions of one entity**?                                                      | → **State Machine** (§13)      |
| Does the diagram show **structural relationships** between entities (no time axis)?                                  | → **Actor Relationship** (§14) |
| Does the diagram show **separate autonomous processes** (different organizations or systems) that exchange messages? | → **BPMN Collaboration** (§15) |

> **Rule**: If none of the above fits cleanly, decompose the intent into
> sub-diagrams, each of which maps to one type. Do not combine types in a
> single diagram.

### §0b. Pool vs Lane vs Swimlane — Concept Clarification

These three terms are often confused. Use the precise definition every
time:

| Term              | Definition                                                                                              | Scope                     |
| ----------------- | ------------------------------------------------------------------------------------------------------- | ------------------------- |
| **Pool**          | Represents one autonomous process — one organization, one system, one agent. Has its own sequence flow. | Top-level container       |
| **Lane**          | A sub-division _inside_ a Pool. Represents a role, department, or actor _within_ that single Pool.      | Inside a Pool             |
| **Swimlane**      | A generic visual band in a diagram. In draw.io, often used loosely to mean a Lane row.                  | Visual term, not semantic |
| **Participant**   | BPMN synonym for Pool. The outer container in a collaboration diagram.                                  | Top-level container       |
| **Message Flow**  | A dashed arrow between two _different_ Pools. Crosses Pool boundaries.                                  | Cross-Pool connector      |
| **Sequence Flow** | A solid arrow inside _one_ Pool. Stays within Pool boundaries.                                          | Intra-Pool connector      |

**Decision rule**:

- Same organization, different roles → multiple **Lanes** inside **one
  Pool**
- Different organizations or autonomous systems → multiple **Pools** (use
  §15)
- Current Appoint AI swimlane diagrams use multiple Lanes inside an
  implicit single Pool. This is correct and intentional for
  single-organization plugin build workflows.

### §0c. When to Create Multiple Diagrams

Create separate diagram files (not pages) when:

- The subject matter changes (e.g., process flow AND entity relationships)
- The diagram becomes unreadable at standard zoom with all content on one
  page
- Different teams own different diagram files

Use separate `<diagram>` pages inside one `.drawio` file when:

- Content is related (e.g., overview diagram + detail drill-down)
- All pages share the same actor set

---

## 1. Define the Semantic Model First

Before touching any XML, identify and document the following:

### 1a. Actors

- List every participant, system, or decision authority with a short,
  unambiguous name.
- Orchestrator actors **must** start with `Appoint` (e.g.,
  `AppointOrchestrator`).
- Assign each actor exactly one classification:
  - **Top-level participant** — owns a lane
  - **Internal subprocess owner** — nested within a lane
  - **External participant** — sibling lane, never parented inside another
    lane
  - **Note / reference only** — annotation, not structural

### 1b. Interaction Types

For each relation between actors, classify as one of:

| Type                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| Internal flow        | Stays within one actor's lane                   |
| Handoff              | Cross-lane delegation from one actor to another |
| External interaction | Request or response to/from an external actor   |
| Escalation           | Priority override to a different actor          |
| Decision response    | Branch outcome from a gateway node              |

---

## 2. Define Relations Before Creating Connectors

For each intended connector, define the following before drawing it:

| Attribute        | Description                                         |
| ---------------- | --------------------------------------------------- |
| Source actor     | The actor or shape sending the signal               |
| Target actor     | The actor or shape receiving the signal             |
| Semantic meaning | What this connection represents                     |
| Direction type   | Internal / cross-lane / external                    |
| Label            | Explicit text for ambiguous branches or escalations |

Only add a connector after its meaning is fully defined. Label all
ambiguous branches, escalations, and request/response pairs explicitly.

---

## 3. Map the Semantic Model to Containers

- **Top-level participants** → sibling top-level swimlane rows at the page
  root.
- **Internal work** → children of the correct participant's lane.
- **External participants** → sibling lanes, never children of an internal
  lane.
- **Internal responsibility splits** → use nested lanes inside the
  participant container.
- **Annotations** → notes and text cells only; never use as structural
  containers.

Choose containers by semantic meaning first, visual convenience second.

---

## 4. Swimlane Layout Standard

Use the **horizontal actor-lane** layout for all plugin build process
diagrams:

- One horizontal swimlane row (`swimlane;horizontal=0`) per top-level
  actor.
- Process flows **left to right** within each lane.
- Cross-lane connectors represent handoffs, requests, or responses between
  actors.
- Internal work within an actor is ordered left to right, top to bottom
  only when needed.
- External actors are sibling lanes at the same nesting level as internal
  actors.

### 4a. Standard Lane Order (top to bottom)

For the plugin build process, use this canonical lane order:

| Position | Actor | Role |
| --- | --- | --- |
| 1 | User | Initiates request, approves result |
| 2 | AppointOrchestrator (parent) | Plans, delegates, synthesizes |
| 3 | AppointChildOrchestrator (optional) | Owns one bounded workstream when needed |
| 4 | AppointDrawioWorker | Authors and validates the process.drawio diagram |
| 5 | AppointScaffoldWorker | Scaffolds and validates the plugin |
| 6+ | Additional workers | Domain-specific implementation |

Add domain-specific worker lanes below as needed. Omit the child-orchestrator lane when no bounded workstream owner is required.

### 4b. Standard Color Coding

| Actor Role          | Fill Color | Stroke Color | Fill Hex  |
| ------------------- | ---------- | ------------ | --------- |
| User                | Yellow     | Gold         | `#fff2cc` |
| Parent orchestrator | Blue       | Steel blue   | `#dae8fc` |
| Child orchestrator  | Light blue | Slate blue   | `#e1f0ff` |
| Worker (design)     | Green      | Sage         | `#d5e8d4` |
| Worker (build)      | Purple     | Lavender     | `#e1d5e7` |
| External system     | Light grey | Charcoal     | `#f5f5f5` |

### 4c. External Review Loop

If an external actor performs meaningful work before returning a result:

- Represent that work **inside the external actor's own lane**.
- Do not collapse the external actor to a single response node.
- Return the result as an explicit response connector from the external
  actor's lane back to the requester or orchestrator lane.

### 4d. Decision Branch Visibility

For every decision gateway (rhombus):

- Each branch **must** have an explicit readable label (e.g.,
  `Yes — Proceed` / `No — Revise`).
- Position branch labels away from shape edges, connector elbows, and
  container titles.
- Prefer dedicated connector labels over implied direction alone.

### 4e. Cross-Lane Connector Routing

- Route paired request/response connectors through separate visual
  corridors so they do not overlap.
- Use different anchor sides or elbow heights for paired connectors.
- Keep cross-lane connectors away from gateway labels and unrelated task
  boxes.
- Revision loops that route back left must use explicit waypoints to avoid
  ambiguous paths.

---

## 5. Draw.io XML Structure and Conventions

### 5a. File Skeleton

Every `process.drawio` file must follow this structure:

```xml
<mxfile host="app.diagrams.net" modified="YYYY-MM-DDTHH:MM:SS.000Z"
        agent="Appoint AI Plugin Builder" version="21.0.0">
  <diagram id="plugin-build-workflow" name="Plugin Build Workflow">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1" page="1"
                  pageScale="1" pageWidth="1654" pageHeight="1169"
                  math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- title, lanes, shapes, edges -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

### 5b. Required Cell IDs — Semantic Naming Convention

Use human-readable semantic IDs, never auto-generated UUIDs:

| Shape type       | ID convention          | Example          |
| ---------------- | ---------------------- | ---------------- |
| Lane             | `lane<ActorName>`      | `laneOrch`       |
| Task / activity  | `task<PascalName>`     | `taskPlanBuild`  |
| Decision gateway | `gate<PascalName>`     | `gateReview`     |
| Start event      | `startEvent`           | `startEvent`     |
| End event        | `endEvent`             | `endEvent`       |
| Edge             | `e<sequential-number>` | `e1`, `e2`, `e3` |

### 5c. Standard Shape Styles

| Shape            | Required style attributes                                          |
| ---------------- | ------------------------------------------------------------------ |
| Swimlane row     | `swimlane;startSize=30;horizontal=0;fontStyle=1;fontSize=12`       |
| Task / activity  | `rounded=1;arcSize=10`                                             |
| Decision gateway | `rhombus`                                                          |
| Start event      | `ellipse` with green fill (`#00897B`), white text, no stroke width |
| End event        | `ellipse;strokeWidth=3` with red fill (`#AE4132`), white text      |

### 5d. Standard Dimensions

| Shape             | Width | Height | Notes                     |
| ----------------- | ----- | ------ | ------------------------- |
| Swimlane row      | 1560  | 150    | All lanes identical width |
| Task / activity   | 140   | 60     |                           |
| Decision gateway  | 80    | 60     |                           |
| Start / End event | 60    | 60     |                           |

Shapes must be **vertically centered** in their lane:
`y = (laneHeight − shapeHeight) / 2`.  
For the standard 150px lane with a 60px shape: `y = 45`.

### 5e. Lane Positioning

Stack lanes consecutively with no gaps:

| Lane index | `y` in page coordinates | Formula        |
| ---------- | ----------------------- | -------------- |
| 1          | 40                      | `40 + 0 × 150` |
| 2          | 190                     | `40 + 1 × 150` |
| 3          | 340                     | `40 + 2 × 150` |
| 4          | 490                     | `40 + 3 × 150` |
| N          | `40 + (N−1) × 150`      |                |

All lanes share `x=40` and `width=1560`.

### 5f. Edge Parent Rules

| Edge type       | `parent` value | Reason                                    |
| --------------- | -------------- | ----------------------------------------- |
| Same-lane edge  | Lane cell ID   | Stays in local coordinate system          |
| Cross-lane edge | `"1"` (page)   | Common ancestor of both source and target |

Always use `edgeStyle=orthogonalEdgeStyle;rounded=0` on all edges.  
Specify `exitX`, `exitY`, `entryX`, `entryY` and `<Array as="points">`
waypoints on any revision loop or non-obvious routing path.

---

## 6. Validate Geometry Explicitly

Before finalizing, verify:

- Each shape's `x + width ≤ parent lane width` and
  `y + height ≤ parent lane height`.
- All lanes share the same `width` and `x` values.
- No external actor lane is positioned so it appears to be inside another
  lane (no y-overlap between sibling lanes).
- All lanes are stacked with no gap between consecutive lane bottom/top
  edges.
- Start events and end events are at consistent horizontal positions across
  the page.

---

## 7. Validate Connector Intent

- Internal flow connectors stay visually within the owning lane.
- Cross-lane connectors represent genuine actor handoffs, requests, or
  responses only.
- Each connector points to the semantically correct source and target cell.
- Paired request/response connectors do not overlap or read as one line.
- Revision loops are routed back-left with explicit waypoints and are
  visually distinct from forward-progress connectors.
- Recheck connector routing after any shape has been moved.

---

## 8. Validate Semantic Rendering

Do not stop at well-formed XML. After writing the diagram, compare the
intended model against the rendered layout:

1. Actor list → lane list: every actor has exactly one lane, no lane is
   missing.
2. Relation list → connector list: every defined relation has exactly one
   connector.
3. Each shape is inside the correct lane container.
4. Flow direction reads naturally left to right.
5. Decision branches are labeled and unambiguous on the rendered canvas.
6. Revision loops are visually distinct from forward connectors.
7. If rendered meaning and XML structure disagree, fix the XML before
   finishing.

---

## 9. Minimum Completion Checklist — Swimlane Process Diagram

Before a swimlane process diagram is considered complete and merged:

- [ ] Diagram type was explicitly selected using §0a before authoring began
- [ ] Pool/Lane choice was justified per §0b (single-org Lanes, not
      multi-Pool)
- [ ] Semantic model documented: actors named, classified, and relations
      defined
- [ ] Each top-level actor has its own swimlane row
- [ ] External actors are sibling lanes, not nested inside internal lanes
- [ ] Decision gateways have explicit labels on every branch
- [ ] All cross-lane connectors represent genuine actor handoffs or
      interactions
- [ ] All loop patterns are classified (revision / iterative / retry /
      nested) per §10
- [ ] Revision loops route back clearly without overlapping
      forward-progress connectors
- [ ] Iterative and retry loops have explicit boundary markers
- [ ] All shapes fit within their lane boundaries (geometry verified per
      §6)
- [ ] All connector source/target IDs point to the correct semantic
      endpoints
- [ ] Lane colors follow the standard color coding table (§4b)
- [ ] Cell IDs follow the semantic naming convention (§5b)
- [ ] Rendered layout visually matches the intended actor-lane semantic
      model
- [ ] File is saved under `plans/drawio/`

---

## §10. Loop and Complex Scenario Patterns

The swimlane diagram supports four distinct loop patterns. Identify the
correct pattern before drawing any back-arc connector.

### §10a. Revision Loop (most common)

**Intent**: A reviewer rejects an artefact and sends it back for rework.

**Pattern**:

```text
taskProduce → gateReview → [No — Revise] → taskProduce (back-arc)
                         → [Yes — Proceed] → next task
```

**XML rules**:

- Back-arc edge: `parent="1"` (page root, cross-lane)
- Use `edgeStyle=orthogonalEdgeStyle` with `<Array as="points">` waypoints
- Route the back-arc **below** or **above** the lane stack to avoid overlap
  with forward connectors
- Label the back-arc explicitly: `No — Revise`
- Label the forward branch: `Yes — Proceed`

**ID convention**: `eRevise<TargetTask>` e.g. `eReviseAuthorDrawio`

---

### §10b. Iterative Loop (for-each)

**Intent**: A set of items is processed one by one until the set is
exhausted.

**Pattern**:

```text
gateLoopStart[More items?] → taskProcessItem → gateLoopEnd[Done?]
      ↑_______________back-arc (No — Next item)___↑
      gateLoopEnd → [Yes — All done] → next task
```

**XML rules**:

- Use two gateways: one entering the loop body, one exiting
- Label entry gateway: `More items?` / `Yes — Next` / `No — Skip all`
- Label exit gateway: `All done?` / `Yes — Continue` / `No — Next item`
- Back-arc routes left, from exit gateway back to entry gateway
- Use `edgeStyle=orthogonalEdgeStyle` with explicit waypoints through a
  routing corridor

**ID convention**: `gateLoopStart<Name>`, `gateLoopEnd<Name>`,
`eLoopBack<Name>`

---

### §10c. Retry Loop (poll-until)

**Intent**: A task retries until a success condition is met (polling, error
recovery).

**Pattern**:

```text
taskAttempt → gateSuccess[Success?] → [Yes — Proceed] → next task
                  ↑                  → [No — Retry] ↙
                  └────────back-arc──────────────────┘
```

**XML rules**:

- Single gateway after the task
- Label retry branch: `No — Retry` (or `Failed — Retry`)
- Label exit branch: `Yes — Proceed` (or `Success — Continue`)
- Back-arc returns to the _task_ (not the gateway)
- Add a **max-retry annotation** (text note) near the gateway:
  `Max N retries`
- Route back-arc through a dedicated corridor above the task

**ID convention**: `gateRetry<Name>`, `eRetryBack<Name>`

---

### §10d. Nested Loop

**Intent**: An outer loop containing an inner loop (e.g., for each agent,
retry each step).

**Rules**:

- Use the outer loop pattern from §10b for the outer boundary
- Use §10a or §10c for the inner loop
- Route outer back-arc through a **separate vertical corridor** from the
  inner back-arc
- Annotate each loop level with a label band: `Outer loop: per agent` /
  `Inner loop: per step`
- Keep inner and outer back-arcs visually non-overlapping; use separate `y`
  offsets in waypoints

**ID convention**: prefix inner loop IDs with `inner` — e.g.,
`gateInnerLoopStart<Name>`

---

## §11. Extended Shape Palette

The current swimlane instructions cover only basic shapes. This section
extends the palette for all supported diagram types. Use only shapes from
this table; do not invent styles.

### §11a. BPMN Task Types (inside lanes/pools)

| Task type     | Shape / Style                                                                         | Use when                                     |
| ------------- | ------------------------------------------------------------------------------------- | -------------------------------------------- |
| Abstract task | `rounded=1;arcSize=10`                                                                | Generic work step (default)                  |
| User task     | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=userTask`     | Human performs the step                      |
| Service task  | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=serviceTask`  | Automated/API call                           |
| Script task   | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=scriptTask`   | Code execution                               |
| Loop task     | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=loopStandard` | Task repeats (attach to standard task shape) |
| Sub-process   | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=subProcess`   | Collapsed detail block                       |

### §11b. BPMN Event Types

| Event type     | Style                                                                                      | Placement            |
| -------------- | ------------------------------------------------------------------------------------------ | -------------------- |
| Start (plain)  | `ellipse;fillColor=#00897B;strokeColor=#005662;fontColor=#ffffff`                          | Process entry        |
| End (plain)    | `ellipse;strokeWidth=3;fillColor=#AE4132;strokeColor=#6d0000`                              | Process exit         |
| Intermediate   | `ellipse;strokeWidth=2`                                                                    | Mid-flow event       |
| Timer start    | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=timer;isLooping=1` | Scheduled trigger    |
| Error boundary | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=errorBoundary`     | Attached to task     |
| Message start  | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=message`           | Triggered by message |

### §11c. Gateway Types

| Gateway type    | Style                                                                          | Semantics                            |
| --------------- | ------------------------------------------------------------------------------ | ------------------------------------ |
| Exclusive (XOR) | `rhombus` with `X` label marker                                                | Exactly one branch taken             |
| Parallel (AND)  | `rhombus` with `+` label marker                                                | All branches taken simultaneously    |
| Inclusive (OR)  | `rhombus` with `O` label marker                                                | One or more branches taken           |
| Event-based     | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=event` | Branch taken based on event received |

Use the standard `rhombus` shape for the exclusive gateway (most common).
Add the semantic marker as a label character to make the type unambiguous.

### §11d. Sequence Diagram Shapes

| Shape             | Style                                                  | Notes                               |
| ----------------- | ------------------------------------------------------ | ----------------------------------- |
| Lifeline header   | `rounded=1;arcSize=10`                                 | Actor box at top                    |
| Lifeline stem     | `dashed=1;endArrow=none`                               | Vertical dashed line                |
| Activation box    | `shape=mxgraph.uml.activation`                         | Thin rect on lifeline during active |
| Sync message      | `edgeStyle=orthogonalEdgeStyle;endArrow=block`         | Solid arrow, filled arrowhead       |
| Async message     | `edgeStyle=orthogonalEdgeStyle;endArrow=open`          | Solid arrow, open arrowhead         |
| Return message    | `edgeStyle=orthogonalEdgeStyle;dashed=1;endArrow=open` | Dashed arrow                        |
| Combined fragment | `swimlane;startSize=20;fontStyle=1`                    | alt/loop/opt frame                  |

### §11e. State Machine Shapes

| Shape         | Style                                                         | Notes                    |
| ------------- | ------------------------------------------------------------- | ------------------------ |
| State         | `rounded=1;arcSize=50`                                        | Pill-shaped              |
| Initial state | `ellipse;fillColor=#000000;strokeColor=#000000`               | Filled black circle      |
| Final state   | `ellipse;strokeWidth=3;fillColor=#000000;strokeColor=#000000` | Circle with thick ring   |
| Transition    | `edgeStyle=orthogonalEdgeStyle;endArrow=block` with label     | Trigger [guard] / action |
| Choice pseudo | `rhombus`                                                     | Dynamic choice point     |

### §11f. Actor Relationship Shapes

| Shape          | Style                                                                | Notes                  |
| -------------- | -------------------------------------------------------------------- | ---------------------- |
| Entity / Actor | `rounded=1;arcSize=10`                                               | Node box               |
| Association    | `edgeStyle=orthogonalEdgeStyle;endArrow=none`                        | Undirected line        |
| Directed assoc | `edgeStyle=orthogonalEdgeStyle;endArrow=open`                        | One-way relationship   |
| Aggregation    | `edgeStyle=orthogonalEdgeStyle;endArrow=open;startArrow=diamondThin` | "has-a" hollow diamond |
| Composition    | `edgeStyle=orthogonalEdgeStyle;endArrow=open;startArrow=diamond`     | "owns" filled diamond  |
| Cardinality    | Edge label e.g. `1`, `0..*`, `1..*`                                  | Place at both ends     |

---

## §12. Sequence Diagram Layout

Use the sequence diagram when the **order of messages over time** is the
primary concern and the actors are already well-understood (do not
redocument what is in the swimlane diagram).

### §12a. Layout Rules

- Flow direction: **top to bottom** (time axis is vertical)
- Lifelines: one per actor, arranged left to right by first-send order
- Page dimensions: `pageWidth="1169" pageHeight="1654"` (portrait
  orientation)
- Lifeline header: `width=120, height=40`, centered at `x = 40 + (i × 200)`
- Lifeline stem: `x = header.x + 60`, `y = header.y + 40`, extends to page
  bottom minus 40

### §12b. Message Routing Rules

- Synchronous call → solid arrow with filled arrowhead, label =
  method/event name
- Asynchronous call → solid arrow with open arrowhead
- Return / response → dashed arrow, label = return value or status
- Self-call → loop-back arrow returning to the same lifeline
- Always label every arrow. Unlabeled arrows are ambiguous.

### §12c. Combined Fragments

For loops, alternatives, and optional blocks in sequence diagrams:

| Fragment | Label                           | Meaning                         |
| -------- | ------------------------------- | ------------------------------- |
| `loop`   | `loop [condition]`              | Repeating block                 |
| `alt`    | `alt` with `[condition]` guards | Mutually exclusive alternatives |
| `opt`    | `opt [condition]`               | Optional block                  |
| `par`    | `par`                           | Parallel execution              |
| `break`  | `break [condition]`             | Break out of enclosing fragment |

Use a `swimlane` container with `startSize=20` for each combined fragment.

### §12d. Completion Checklist — Sequence Diagram

- [ ] Diagram type selected: Sequence
- [ ] Lifelines represent distinct actors, left-to-right by first-send
      order
- [ ] Every arrow is labeled with message name or return value
- [ ] Synchronous calls have filled arrowheads; async calls have open
      arrowheads; returns are dashed
- [ ] All loops and alternatives use combined fragment containers
- [ ] Time order reads top to bottom without crossing arrows where
      avoidable
- [ ] Cell IDs use naming convention: `lifeline<Actor>`, `msg<N>`,
      `frag<Name>`
- [ ] File saved under `plans/drawio/`

---

## §13. State Machine Diagram Layout

Use the state machine when showing the **lifecycle of a single entity** —
its states and the events or conditions that cause transitions.

### §13a. Layout Rules

- Flow direction: **left to right** (matches swimlane convention) or
  radially for complex graphs
- One page per entity (do not mix multiple entities' state machines on one
  page)
- Initial state at far left, terminal states at far right
- Page dimensions: `pageWidth="1169" pageHeight="827"` (landscape)

### §13b. State Naming and Transitions

- State names: `PascalCase` (e.g., `Initializing`, `Active`, `Suspended`,
  `Terminated`)
- Transition labels: `trigger [guard] / action` on the edge (omit sections
  that don't apply)
  - Example: `userInvokes [isReady] / loadContext`
- Self-transitions: loop arrow back to the same state; place label above
  the loop
- History states: use `(H)` annotation inside the state box

### §13c. Guard Conditions

Every non-trivial branch from a state **must** have an explicit guard:

- `[guard]` on the edge label, placed at mid-arc
- At each choice pseudo-state, every outgoing edge must have a guard, and
  the guards must be mutually exclusive and exhaustive (together cover all
  outcomes)

### §13d. Completion Checklist — State Machine

- [ ] Diagram type selected: State Machine
- [ ] One entity's lifecycle per diagram
- [ ] Initial state (filled circle) present
- [ ] At least one terminal/final state present
- [ ] Every transition has a label (trigger [guard] / action as applicable)
- [ ] Guards at branch points are mutually exclusive and exhaustive
- [ ] Self-transitions are visible and labeled
- [ ] Cell IDs: `state<Name>`, `stateStart`, `stateEnd`, `t<N>`
      (transitions)
- [ ] File saved under `plans/drawio/`

---

## §14. Actor Relationship Diagram Layout

Use the actor relationship diagram when showing **structural relationships
between entities** with no time axis — who is connected to whom, what owns
what, what depends on what.

### §14a. Layout Rules

- No strict flow direction; arrange to minimize crossing edges
- Group strongly connected entities spatially
- Central / most-connected entity near diagram center
- Use consistent spacing: minimum 80px gap between adjacent entity boxes
- Page dimensions: `pageWidth="1169" pageHeight="827"` (landscape) or
  square as needed

### §14b. Relationship Classification

Before drawing any edge, classify it from this table:

| Relationship   | Arrow style                         | Label format                     |
| -------------- | ----------------------------------- | -------------------------------- |
| Association    | Plain line, no arrowhead            | Verb phrase: `uses`, `has`       |
| Directed assoc | Open arrowhead at target            | Verb phrase: `calls`, `triggers` |
| Aggregation    | Hollow diamond at source            | `0..*` / `1` cardinality labels  |
| Composition    | Filled diamond at source            | `1` / `1..*` cardinality labels  |
| Dependency     | Dashed line, open arrowhead         | `«uses»`, `«depends»`            |
| Inheritance    | Solid line, hollow triangle at base | No label needed                  |

Always show cardinality at **both** ends of associations and aggregations.

### §14c. Completion Checklist — Actor Relationship

- [ ] Diagram type selected: Actor Relationship
- [ ] All relationships classified before drawing
- [ ] Cardinality shown at both ends of every association
- [ ] No time-ordered flow arrows (use swimlane or sequence for those)
- [ ] Crossing edges minimized by spatial grouping
- [ ] Cell IDs: `entity<Name>`, `rel<N>`
- [ ] File saved under `plans/drawio/`

---

## §15. BPMN Collaboration Diagram (Multi-Pool)

Use the BPMN Collaboration diagram when showing **two or more autonomous
processes** (different organizations, systems, or agents with independent
sequence flows) that exchange messages.

> **Do not use** this diagram type for internal Appoint AI workflows — use
> §4 swimlane instead. Use this type when: VS Code Extension ↔ Appoint AI
> Cloud ↔ External API, or when two teams own separate process flows that
> interact via defined interfaces.

### §15a. Pool Layout Rules

- Each Pool is a top-level swimlane container with its own header
- Lanes inside a Pool represent roles/actors within that organization
- Pools are stacked vertically with a 20px gap between them
- **Sequence Flows** (solid arrows, `parent = pool cell ID`) stay inside
  one Pool
- **Message Flows** (dashed arrows, `parent = "1"` page root) cross Pool
  boundaries
- Message Flows must start and end at Pool/Lane boundaries (not at internal
  task centers)

### §15b. XML Structure for Multi-Pool

```xml
<!-- Pool 1 -->
<mxCell id="pool1" value="VS Code Extension" style="pool;..." vertex="1" parent="1">
  <mxGeometry x="40" y="40" width="1560" height="200" as="geometry" />
</mxCell>
<mxCell id="lane1a" value="Agent" style="swimlane;startSize=30;horizontal=0;..." vertex="1" parent="pool1">
  <mxGeometry x="0" y="30" width="1560" height="85" as="geometry" />
</mxCell>

<!-- Pool 2 -->
<mxCell id="pool2" value="External API" style="pool;..." vertex="1" parent="1">
  <mxGeometry x="40" y="260" width="1560" height="150" as="geometry" />
</mxCell>

<!-- Message Flow (cross-pool, parent=1) -->
<mxCell id="mf1" value="API Request" style="edgeStyle=orthogonalEdgeStyle;dashed=1;endArrow=open;" edge="1" parent="1" source="taskCallAPI" target="taskReceiveRequest">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

### §15c. ID Convention for Multi-Pool

| Shape type       | ID convention              | Example           |
| ---------------- | -------------------------- | ----------------- |
| Pool             | `pool<OrgName>`            | `poolVSCode`      |
| Lane inside Pool | `lane<PoolAbbr><RoleName>` | `laneVSCodeAgent` |
| Message Flow     | `mf<N>`                    | `mf1`, `mf2`      |
| Sequence Flow    | `e<N>` (pool-scoped)       | `e1`, `e2`        |

### §15d. Completion Checklist — BPMN Collaboration

- [ ] Diagram type selected: BPMN Collaboration
- [ ] Each autonomous process has its own Pool
- [ ] Lanes inside Pools represent roles (not separate organizations)
- [ ] Sequence Flows are children of their Pool cell
- [ ] Message Flows are children of page root (`parent="1"`)
- [ ] Every Message Flow is labeled with the interface/message name
- [ ] No Sequence Flow crosses a Pool boundary
- [ ] Cell IDs follow multi-pool naming convention (§15c)
- [ ] File saved under `plans/drawio/`
