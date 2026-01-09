# 1. Goal of the DSL

We are developing a **domain-specific language (DSL)** that enables the **atomization, validation, and traceability of requirements (AFOs)** in regulated software systems and above.

This language is designed to:

- be **human- and machine-readable** (structured text or JSON),
- represent **syntactically unambiguous and verifiable requirements**,
- be **testable, versionable, and auditable** (via Git integration),
- support **automatic validation and auditing** (CI/CD),
- and **semantically represent governance rules, negative rules, and exclusions**.

This documentation reflects the current architecture direction.

---

# 2. Design Principles

| No. | Principle | Description |
|:---:|:-----------|:-------------|
| 1 | **Human readability** | Text structure and indentation must be visually logical. |
| 2 | **Machine validation** | Unambiguous grammar and reserved keywords. |
| 3 | **Parser-friendly grammar** | Formally defined, context-free (CFG-based). |
| 4 | **Extensibility** | Syntax and semantics are modular and adaptable. |
| 5 | **Toolchain integration** | Integrated with Git, CI/CD, and audit pipelines. |
| 6 | **Java-free implementation** | Focus on Deno/TypeScript or Rust environments. |
| 7 | **Rule-based prohibitions** | “Must not” and “May not” constructs for governance. |
| 8 | **Central traceability** | Full audit trail and traceability of all rule changes. |
| 9 | **Security by Design** | Validation, parsing, and data handling must guarantee protection against injection, malformed input, and misuse. The DSL never executes input, never interprets code, and enforces deterministic, safe processing at all times. |

---

- **dsl-core is the normative engine**: parsing + deterministic validation of atomic requirements.
- Collaboration, orchestration and workflows (CI, jobs, ledgers, portals) live as **adapter layers** on top of the engine.

This separation is intentional. It keeps dsl-core:
- **auditable** (minimal surface, deterministic behavior),
- **portable** (usable in public sector, enterprises, research),
- **composable** (multiple tools can consume the same contracts),
- **future-proof** (UI/platform decisions do not leak into the core).

### Scope Boundary (Non-goals of dsl-core)

dsl-core does **not** implement:
- platform UI,
- collaboration portals,
- user management, permissions, or billing,
- job marketplaces or governance workflows.

dsl-core **does** implement:
- a stable DSL parsing model (RequirementAtom),
- deterministic rule-based validation,
- **evidence artifacts** (machine-readable outputs suitable for audits).

---

## Public Contracts as First-Class Artifacts

dsl-core publishes **public contracts** as JSON Schemas under:

- `/schemas/`

These contracts define the canonical outputs of the engine and are intended to be consumed by:
- CI pipelines,
- reporting tools,
- orchestration layers (e.g., Rock the Project adapters),
- external validators and audit tooling.

### Report Contract (canonical evidence)

The core evidence artifact is a canonical **validation report**:

- `schemas/report.schema.json`

This report is designed to be:
- deterministic,
- schema-validatable,
- stable across internal refactorings,
- usable as an audit attachment (“Evidence by Design”).

Contract changes follow SemVer rules:
- breaking report format changes require a major version bump of the report contract.

---

## Reference CLI as Adapter (Deno-only)

The repository contains a **reference CLI** located at:

- `/cli/`

The CLI is an adapter layer:
- it consumes the engine API (`src/mod.ts`),
- it provides developer UX (commands, flags, file IO, exit codes),
- it must not introduce dependency cycles back into the engine.

The CLI is intentionally Deno-first and avoids npm-based packaging by default.

### Exit Code Semantics

The CLI uses stable exit codes for CI integration:

- `0` → no parse errors and all validations passed
- `1` → validations failed (rule violations exist)
- `2` → parse errors or usage/config errors

---

## Determinism and Trust Model

dsl-core is built for regulated environments. Therefore:

- **Validation** is deterministic (no network calls, no hidden runtime behavior).
- Output formats are defined via explicit JSON Schemas.
- **Evidence** artifacts must be reproducible from the same inputs.

This is the foundation for:
- **auditability**,
- **long-term maintainability**,
- meaningful **automation** (CI gates),
- a credible **collaboration** / funding story built on **transparent** artifacts.

---

## Extension Strategy (stable core, flexible adapters)

dsl-core supports extensions by:
- adding new rules (without breaking existing contracts),
- using `extensions` fields in outputs where appropriate,
- keeping adapters free to enrich reports, generate jobs, or attach governance metadata
  **without changing the normative engine**.
