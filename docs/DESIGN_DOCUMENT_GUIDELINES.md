# Design Document Guidelines

Guidelines for writing detailed design documents that conform to IEEE 1016-2009 (Software Design Descriptions) and IEEE 29148-2018 (Requirements Engineering). Derived from the Gaggiuino design document experience.

---

## Document Structure

A compliant design document has four major parts, in this order:

```
1. Front Matter        (purpose, scope, notation, stakeholders, glossary, references)
2. Design Views        (one section per IEEE 1016 viewpoint you select)
3. Requirements        (IEEE 29148 — safety, functional, security, data, performance, usability)
4. Verification        (test plan, traceability matrix)
```

Everything else (current-state analysis, FMEA, standards review, decisions log) is supplementary and goes after these four.

---

## Part 1: Front Matter

Every design document must open with these subsections. None are optional.

### 1.1 Purpose Statement

One sentence: what this document is, who reads it, and how it should be used.

> **Example:** "This document specifies the design and requirements for the Gaggiuino firmware rewrite, serving as the primary reference for firmware contributors, hardware builders, and web app developers."

### 1.2 Scope

What the document covers and — just as importantly — what it does not.

### 1.3 Project Objective

One sentence stating the goal in outcome terms, not implementation terms.

> **Example:** "Provide an open-source, single-MCU firmware for the Gaggia Classic that eliminates the dual-MCU architecture while maintaining safety parity with the stock machine."

### 1.4 Operational Concept (Day-in-the-Life)

A numbered walkthrough of a typical usage session, from power-on to power-off. This grounds every subsequent requirement in real user behavior. Keep it to 10-15 steps.

### 1.5 Notation

Declare every design language used in the document. Typical set:

- ASCII diagrams — architecture, wiring, state machines
- Code snippets (language) — interface definitions, algorithms
- JSON / JSON Schema — data models
- Tables — requirements, FMEA, test plan
- Prose — rationale and analysis

### 1.6 Stakeholder Concerns and Document Map

A table mapping each stakeholder role to:
- Their primary concerns (what they need to know)
- The sections of this document that address those concerns

| Stakeholder | Primary Concerns | Relevant Sections |
|-------------|-----------------|-------------------|
| Role A | What they care about | 3, 5, 8 |
| Role B | What they care about | 4, 7, 12 |

### 1.7 User Characteristics

Table of user roles with description and technical level.

### 1.8 Glossary

Every domain-specific term, abbreviation, or non-obvious concept used in the document. Include:
- Hardware component names (ICs, sensors, actuators)
- Protocol names and abbreviations
- Domain concepts that have precise meaning in this system
- Return value conventions (e.g., NAN as fault indicator)

**Rule of thumb:** If a reader outside the core team would need to look it up, define it.

### 1.9 References

Numbered table of all external documents referenced. Each entry needs:
- ID (e.g., [R01])
- Linked title
- Why it's relevant (one phrase)

Include: datasheets, framework docs, safety standards, upstream repos, specification standards (IEEE, IEC), and third-party library docs.

### 1.10 Table of Contents + Reading Guide

After the TOC, add 2-3 sentences explaining the document's structure — which sections are current-state analysis, which are future-state design, which are requirements, which are verification. Readers should know where to start without reading everything.

---

## Part 2: Design Views (IEEE 1016)

IEEE 1016 defines 10 design viewpoints. You do not need all 10, but you must **explicitly declare which viewpoints you selected** and produce a design view for each.

### Checklist: Select Your Viewpoints

For each viewpoint below, decide: include or skip. If you skip one, briefly state why (e.g., "Resource viewpoint: not applicable — runs on desktop with abundant resources").

### 2.1 Context Viewpoint

**What it is:** A single diagram showing your system as a black box, all external entities, and the data flows crossing the boundary.

**Must include:**
- System boundary (one box)
- Every external actor (users, hardware, external services, other systems)
- Every data flow crossing the boundary, labeled with: direction, data type, and transport
- External library dependencies shown as a separate group

**Common mistakes:**
- Drawing internal architecture instead of external context
- Omitting physical inputs (switches, sensors) and outputs (actuators, displays)
- Forgetting the power source as an external entity for embedded systems

### 2.2 Composition Viewpoint

**What it is:** How the system is decomposed into modules/components.

**Must include:**
- Module tree (directory structure or package diagram)
- Module annotation table with columns:
  - Module name
  - Runtime owner (which task/process/thread)
  - Pure logic? (testable on host without hardware)
  - Dependencies (other internal modules it imports)

**Common mistakes:**
- Listing files without saying which runtime entity owns them
- Not marking which modules are testable in isolation

### 2.3 Logical Viewpoint

**What it is:** Classes, interfaces, their attributes, operations, and relationships.

**Must include:**
- Interface inheritance (e.g., `TemperatureSensor` → `Max31855Thermocouple`)
- Composition relationships (e.g., ControlTask owns PidController, PhaseProfiler)
- Data types that flow between components

**When to skip:** Pure C projects without object-oriented structure. Replace with "data flow diagram" if more appropriate.

### 2.4 Dependency Viewpoint

**What it is:** Build-time and runtime dependencies.

**Must include:**
- Third-party library table: name, version, license, purpose
- License compatibility statement
- Internal module dependency graph or matrix (if not covered in composition)

**Common mistakes:**
- Listing libraries without versions (causes reproducibility issues)
- Ignoring license compatibility (LGPL, GPL propagation)

### 2.5 Interface Viewpoint

**What it is:** Formal specifications of every boundary where two components meet.

**Must include for each interface:**
- Who produces, who consumes
- Data format (types, schemas, examples)
- Protocol (HTTP, SPI, queue, function call)
- Error handling (error codes, fault values, timeouts)

**For REST APIs specifically:**
- HTTP status codes per endpoint (not just 200 and 401 — also 201, 204, 400, 404, 409, 429, 500)
- Request and response types defined (not just referenced by name)
- Authentication requirements per endpoint
- Rate limiting parameters

**For data schemas specifically:**
- Validation ranges on all numeric fields (min, max)
- String length limits
- Required vs optional fields
- Default values

**Common mistakes:**
- Defining an API that returns `ProfileSummary` without defining what `ProfileSummary` contains
- Omitting error response format
- Not specifying validation bounds (allows invalid data to cause downstream failures)

### 2.6 Structure Viewpoint

**What it is:** Runtime structure — processes, threads, tasks, their configuration.

**Must include (for RTOS / multi-threaded systems):**
- Task table: name, core affinity, priority, stack size, period/trigger, watchdog responsibility
- Priority rationale (why this ordering)
- Stack size rationale (what drives the size)
- ISR latency budgets (if applicable)
- IPC mechanism choice rationale

**Common mistakes:**
- Listing tasks without priorities (causes priority inversion bugs)
- Not specifying stack sizes (causes silent stack overflow crashes)
- Not documenting ISR timing constraints

### 2.7 Interaction Viewpoint

**What it is:** Dynamic behavior — how components interact over time.

**Must include at minimum:**
- One sequence diagram per major user-facing operation (e.g., "make a coffee shot")
- One sequence/timing diagram per timing-critical operation (e.g., ISR → timer → actuator)
- One sequence diagram per complex connection lifecycle (e.g., BLE scan → connect → subscribe)

**Format:** ASCII sequence diagrams, Mermaid, or PlantUML are all acceptable. The key is that the temporal ordering of messages between components is clear.

**Common mistakes:**
- Describing interactions in prose instead of diagrams (prose is ambiguous about ordering)
- Omitting error/timeout paths

### 2.8 State Dynamics Viewpoint

**What it is:** State machines for components with complex lifecycle behavior.

**Must include:**
- Every state from the code (if there's an enum, every enum value must appear in the diagram)
- Every transition, labeled with:
  - **Trigger** — what event causes the transition
  - **Guard** — what conditions must be true
  - **Action** — what happens on transition
- Sub-state machines for nested state behavior (e.g., profile phase progression within BREWING)

**Recommended format:** ASCII state diagram + transition guard table.

**Common mistakes:**
- State diagram shows 6 states but the code enum has 8 (states silently unreachable)
- Transitions not labeled (reader must guess what triggers them)
- No guard conditions (allows impossible transitions like IDLE → BREWING when temperature is below setpoint)

### 2.9 Algorithm Viewpoint

**What it is:** Computational procedures that a developer needs to implement correctly.

**Must include for each non-trivial algorithm:**
- Mathematical formula or pseudocode (not just a prose description)
- Parameter definitions and units
- Edge cases and boundary conditions
- Rationale for the chosen approach (why this form, not another)

**Algorithms that always need specification:**
- Control algorithms (PID, state estimators, filters)
- Interpolation/curve functions
- Timing-critical computations (phase-angle calculations, PWM mappings)
- Prediction/estimation algorithms
- Anything involving calibration or tuning parameters

**Common mistakes:**
- "The system uses a PID controller" — without specifying discrete-time form, anti-windup strategy, or derivative filtering
- "Ease-in curve" — without specifying whether it's quadratic, cubic, or sinusoidal
- Describing thresholds ("if pressure is high, reduce flow") without exact formulas

### 2.10 Resource Viewpoint

**What it is:** How the design maps to physical computing resources.

**Must include:**
- Memory budget (RAM per component, heap headroom requirement)
- Storage layout (flash partitions, filesystem space budget)
- Bus bandwidth analysis (SPI, I2C, UART — can all peripherals run at required rates simultaneously?)
- Wireless coexistence (if using WiFi + BLE or similar)

**Common mistakes:**
- Assuming resources are infinite (LVGL double-buffering exceeds available SRAM)
- Not checking if flash partitions fit (dual OTA + filesystem + NVS in 16MB)
- Not considering bus contention

---

## Part 3: Requirements (IEEE 29148)

### Requirement Categories

Every design document should include these categories (skip only if genuinely not applicable):

1. **Safety Requirements** (REQ-S-*) — for systems that can cause physical harm
2. **Functional Requirements** (REQ-F-*) — what the system does
3. **Security Requirements** (REQ-SEC-*) — for networked/connected systems
4. **Data Requirements** (REQ-D-*) — storage, validation, retention, backup
5. **Performance Requirements** (REQ-P-*) — speed, throughput, resource usage
6. **Usability Requirements** (REQ-U-*) — user experience, accessibility

### Requirement Attributes

Every requirement must have these columns:

| Attribute | Description | Example |
|-----------|-------------|---------|
| **ID** | Unique, prefixed by category | REQ-F-003 |
| **Requirement** | Testable "shall" statement | "The system shall..." |
| **Priority** | P0 (safety-critical), P1 (required), P2 (important), P3 (nice-to-have) | P1 |
| **Rationale** | Why this threshold/value, not another | "170°C is above steam max (165°C) but below thermal fuse (184°C)" |
| **Source** | Where the requirement comes from | IEC 60335-2-15, user need, legacy parity, safety analysis |
| **Verification** | Test ID or method | T-F-003, Visual inspection |

### Writing Good Requirements

**Do:**
- Use "shall" for mandatory requirements, "should" for recommendations
- Make every requirement independently testable
- Include numeric thresholds with units: "within ±1°C", "< 100ms", "≥ 10 Hz"
- State what happens at boundaries: "When storage is full, the oldest record shall be deleted"

**Don't:**
- Write vague requirements: "The system shall be fast" (how fast?)
- Write compound requirements: "The system shall do X and Y" (split into two)
- Write implementation-specific requirements: "The system shall use a FreeRTOS queue" (that's design, not requirement)
- Forget negative requirements: "The system shall NOT expose wifi_password in GET responses"

### Completeness Checklist

For each feature or mode in your system, verify:
- [ ] Is there a functional requirement for it?
- [ ] Is there a performance requirement for its timing/throughput?
- [ ] Is there a test case for it?
- [ ] Is it in the state machine (if it's an operational mode)?

Common gaps found in review:
- Operational modes mentioned in state machine but not in requirements (steam, flush, descale)
- Config persistence behavior not specified (what happens on corruption? on version change?)
- WiFi/network failure behavior not specified (does the system degrade gracefully?)
- Physical input authority not specified (can software override a hardware switch?)
- Security model described in ICD but not captured as numbered requirements
- Data lifecycle not specified (eviction policy, backup, export)

### Performance Requirements Specifically

Every performance requirement needs:
- A numeric value with units
- Rationale for that specific value (derived from hardware limits, control theory, or UX research)
- A test method that can objectively verify it

**Watch for contradictions:** If the requirements say "100 Hz" but the task architecture says "5ms period" (200 Hz), one of them is wrong. Reconcile before the document is final.

---

## Part 4: Verification

### Test Plan

For each test:

| Column | Description |
|--------|-------------|
| **Test ID** | Unique (T-S-001, T-F-001, T-U-001) |
| **Requirements** | Which REQ-* IDs this test covers (one test can cover multiple) |
| **Procedure** | Step-by-step, specific enough for someone else to execute |
| **Pass Criteria** | Objective, measurable (not "works correctly") |

### Test Coverage Rule

**Every requirement must be covered by at least one test.** After writing the test plan, audit:

```
For each REQ-* in section 16:
    Does at least one T-* in section 18 reference it?
    If not → gap. Add a test.
```

### Traceability Matrix

The single most important table in the document. Three columns:

| Requirement | Design Module(s) | Test(s) |
|-------------|------------------|---------|
| REQ-F-001 | control/pid, control/boiler | T-F-001, T-U-001 |
| REQ-F-002 | control/pid, control/dimmer | T-F-002 |

This table answers:
- Forward: "Which code implements this requirement?" → Design Module column
- Backward: "If I change this module, which requirements are affected?" → scan the Design Module column
- Verification: "Is every requirement tested?" → scan for empty Test column cells

**Build this table as you write requirements, not as an afterthought.** It catches gaps immediately.

---

## Common Mistakes (Lessons Learned)

### 1. State machine doesn't match the code enum
If the code has 8 states, the diagram must show 8 states. Audit by comparing the enum values against the diagram labels.

### 2. Requirements without rationale
"Max temperature 170°C" — why not 160 or 180? Without rationale, the next developer will change it arbitrarily. State the reasoning: "Above steam max (165°C), below thermal fuse trip (184°C), provides margin for sensor error."

### 3. API types referenced but not defined
If the API returns `ProfileSummary[]`, define `ProfileSummary` with all its fields. Every type name in the API spec must have a corresponding type definition.

### 4. Validation ranges missing from data schemas
Every numeric field in a JSON schema needs `minimum` and `maximum`. Without them, a profile with `pressure: 999` will be accepted and sent to the actuator.

### 5. Front matter treated as optional
Purpose, scope, notation, stakeholders, glossary, and references are not decoration. They are required by both IEEE 1016 and IEEE 29148. Write them first, not last.

### 6. Algorithms described in prose instead of formulas
"The PID controller computes the output" tells a developer nothing. They need the discrete-time formula, the anti-windup strategy, and the derivative filter coefficient.

### 7. Resource constraints assumed away
Embedded systems have finite RAM, flash, bus bandwidth, and CPU cycles. A resource budget catches problems (like "LVGL needs 300KB but SRAM is 512KB total") before implementation starts.

### 8. Security described in the ICD but not in requirements
If auth is required, there must be numbered REQ-SEC-* requirements that can be tested. An ICD describes *how*; requirements describe *what* and *that*.

---

## Review Checklist

Use this before declaring a design document complete:

### Front Matter
- [ ] Purpose statement present
- [ ] Scope defines what's included AND excluded
- [ ] Project objective in one sentence
- [ ] Operational concept (day-in-the-life scenario)
- [ ] Notation section declares all design languages used
- [ ] Stakeholder table maps roles → concerns → sections
- [ ] Glossary covers all domain terms, abbreviations, conventions
- [ ] References include all external docs (datasheets, standards, libraries)
- [ ] Reading guide after table of contents

### Design Views
- [ ] Viewpoint selection explicitly stated
- [ ] Context diagram shows system boundary + all external entities
- [ ] Module decomposition with task ownership and dependency annotations
- [ ] RTOS/thread configuration table (priority, stack, period, core)
- [ ] Third-party library table (name, version, license)
- [ ] Flash/storage partition layout
- [ ] RAM/resource budget
- [ ] State machine includes ALL states from code, with labeled transitions and guards
- [ ] Algorithm specs use formulas or pseudocode, not just prose
- [ ] At least one sequence diagram per major operation
- [ ] All API types defined (not just named)
- [ ] All schema fields have validation ranges
- [ ] All ICD error handling specified (status codes, fault values)

### Requirements
- [ ] Every operational mode has a functional requirement
- [ ] Security requirements are numbered (not just described in ICDs)
- [ ] Data lifecycle specified (retention, eviction, backup, corruption recovery)
- [ ] Every requirement has: ID, shall-statement, priority, rationale, source, verification
- [ ] No contradictions between requirements and design (e.g., frequency mismatch)
- [ ] Performance requirements have numeric values with units and rationale

### Verification
- [ ] Every requirement mapped to at least one test
- [ ] Test procedures specific enough for someone else to execute
- [ ] Pass criteria are objective and measurable
- [ ] Traceability matrix present: requirement → design module → test
- [ ] All requirement statuses tracked (proposed, approved, implemented, verified)

---

*Based on IEEE 1016-2009 and IEEE 29148-2018. Refined through the Gaggiuino design document (v3.0, 2026-02-16).*
