# support-workflow-spec

## Overview

This repository defines **tool specifications derived from a constitutional specification**.
The constitution is provided by `@tachiiri-library/specifications` and serves as the **non-negotiable upper-layer norm**.

This project is not an implementation repository.

- **AI generates tool specifications**
- **Humans provide intent, direction, and judgment**
- **JSON is the contract**
- **Lint enforces constitutional and operational compliance**
- **Implementation is generated later from validated JSON**

The goal is to establish a **one-way, auditable pipeline**:

```
Constitution
   ↓
Intent (human)
   ↓
Specification JSON (AI-generated, linted)
   ↓
Tool Implementation (AI-generated)
```

---

## Design Principles

### 1. Constitution-first

All tool specifications **must conform** to the constitutional specifications located in:

```
node_modules/@tachiiri-library/specifications/specs/
```

In particular:

- `00_constitution/` defines invariant rules
- `20_operational_semantics/` defines runtime and operational guarantees
- `30_interaction_edges/` defines system boundaries
- `40_service_operations_governance/` defines service governance

No tool may override or contradict these specs.
Deviation is only allowed if explicitly supported by the constitution (e.g. controlled overrides, break-glass).

---

### 2. Clear separation of responsibility

| Layer          | Owner   | Purpose                                   |
| -------------- | ------- | ----------------------------------------- |
| Intent         | Human   | Why the tool exists, boundaries, judgment |
| Spec JSON      | AI      | Concrete, machine-consumable definition   |
| Lint           | Machine | Enforce constitution & consistency        |
| Implementation | AI      | Deterministic output from spec            |

Humans **do not** edit implementation.
AI **does not** invent intent.

---

### 3. Specification unit

Each tool specification is composed of **four fixed domains**:

1. **Identity**
   - Tool identity, versioning, lifecycle, tenant scope

2. **Interface**
   - Inputs, outputs, events, jobs, interaction edges

3. **Policy**
   - AuthN/AuthZ, delegation, exceptions, auditability

4. **Operations**
   - Observability, limits, idempotency, DR, retention, rollout

This structure mirrors the constitution and enables predictable linting and code generation.

---

## Repository Structure (Planned)

This repository is organized to make **intent and certainty explicit to AI**.

```
support-workflow-spec/
├─ intent/
│  └─ <tool-id>/
│     ├─ purpose.md
│     ├─ non_goals.md
│     ├─ actors.md
│     ├─ automation_boundary.md
│     ├─ exceptions.md
│     └─ governance.md
│
├─ spec/
│  └─ <tool-id>.json
│
├─ schema/
│  └─ tool-spec.schema.json
│
├─ lint/
│  ├─ constitutional/
│  ├─ operational/
│  └─ index.ts
│
├─ dist/
│  └─ generated/
│
└─ README.md
```

### intent/

- Written and reviewed by humans
- Expresses **intent, constraints, and decisions**
- References constitutional concepts explicitly
- Serves as the **only source of truth for judgment**

### spec/

- Generated primarily by AI
- Contains **only decided values**
- Must be fully lintable
- No prose, no ambiguity

### schema/

- Defines structural validity
- Enables early failure for incomplete specs

### lint/

- Enforces:
  - Constitutional compliance
  - Cross-field consistency
  - Operational safety guarantees

- Lint failure means **the tool cannot exist**

### dist/

- Output artifacts
- Published via npm
- Consumed by downstream implementation generators

---

## How AI Should Work With This Repository

1. **Read the constitution first**
2. **Read intent documents for a tool**
3. **Generate spec JSON**
4. **Run lint mentally**
5. **Adjust until lint would pass**
6. **Never invent policy not grounded in intent or constitution**

If intent is missing, the correct response is **to ask for clarification**, not to assume.

---

## What This Repository Is NOT

- Not an implementation repository
- Not a playground for experimentation
- Not tolerant of partial or contradictory specs
- Not human-friendly prose-first documentation

This repository treats **specifications as infrastructure**.

---

## First Milestone

The initial goal is to establish:

- One complete tool spec (intent + JSON)
- One constitutional lint rule
- One successful end-to-end validation

Once this loop is stable, scale is trivial.
