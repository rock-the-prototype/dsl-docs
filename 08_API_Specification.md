# 08. API Specification (OpenAPI 3.1)

This document provides the official API specification for the Audit-by-Design DSL Core.  
All endpoints are formally defined using the **OpenAPI 3.1** standard and ensure that the DSL can be integrated safely, deterministically, and reproducibly into external systems, CI/CD pipelines,  
governance engines, and audit frameworks.

The API is intentionally minimal, versioned, and built around the core DSL pipeline:

1. **Normalization** — Convert user input into canonical form  
2. **Parsing** — Apply CFG grammar to produce a structured Requirement Atom  
3. **Schema Validation** — Validate Requirement Atoms against the canonical JSON Schema  
4. **Atomic Pipeline** — End-to-end processing in one request

Together, these APIs offer a fully deterministic contract for sovereign and regulated software environments.

---

## 1. OpenAPI Source File

The complete, machine-readable OpenAPI 3.1 specification is available at:

**`dsl-core/openapi/openapi.yaml`**  
➡️ https://github.com/rock-the-prototype/dsl-core/blob/main/openapi/openapi.yaml

This file defines:

- all HTTP endpoints (`/v1/normalize`, `/v1/parse`, `/v1/validate`, `/v1/atom`)
- request & response schemas
- error formats
- versioning model
- RequirementAtom schema (referencing the canonical JSON Schema)

---

## 2. Versioning Strategy

All API endpoints follow a strict versioning model:

- `/v1/...` marks the first stable API namespace  
- future versions use `/v2`, `/v3`, … without breaking earlier integrations  
- this ensures long-term compatibility for tooling, SaaS providers, governance engines, and audit systems

Each release of the DSL Core will include a matching OpenAPI version to guarantee alignment between documentation, runtime behavior, and compliance expectations.

---

## 3. Motivations & Design Principles

The API is designed according to the DSL’s core principles:

### ✔ Deterministic validation  
Each endpoint produces reproducible output for identical input.

### ✔ Security-by-Design  
All inputs are treated strictly as structured data — never executable logic.  
This prevents code injection, XSS, and RCE at the architectural level.

### ✔ Transparency & auditability  
The OpenAPI file itself becomes part of the audit trail (Git versioning).

### ✔ Extensibility  
Future governance rules, semantic validators, or policy engines can be integrated  
without breaking existing consumers.

### ✔ Digital sovereignty  
Uses open standards only (OpenAPI 3.1, JSON Schema, HTTP, UTF-8).  
No vendor lock-in, no proprietary extensions.

---

## 4. API Overview

The DSL Core exposes four primary API endpoints:

| Endpoint | Purpose | Description |
|---------|----------|-------------|
| `POST /v1/normalize` | Normalization | Produces canonical DSL text (spacing, casing, punctuation) |
| `POST /v1/parse` | Grammar Parsing | Converts DSL text into a Requirement Atom (CFG-based) |
| `POST /v1/validate` | Schema Validation | Validates a Requirement Atom using JSON Schema |
| `POST /v1/atom` | Full Pipeline | normalize → parse → schema-validate in one request |

These endpoints unify the DSL into a robust, auditable processing model.

---

## 5. Compliance Foundations

### JSON Schema  
The canonical machine-readable schema is defined in:  
`/src/schema/requirement.schema.json`

### OpenAPI 3.1  
Used because it is the **only version** aligned with the new JSON Schema vocabulary.

### Kerckhoffs’ principle  
*Security arises from transparency, not obscurity.*  
By publishing a fully documented, verifiable API, the DSL adheres to the same principles  
that modern cryptography, digital identity systems, and sovereign infrastructures rely upon.

---

## 6. Next Steps

Planned additions (tracked in `ROADMAP.md`):

- `/v1/governance/evaluate` — rule-based semantic validation  
- `/v1/rules/diagnostics` — governance engine debug endpoint  
- `/v1/atom/batch` — batch processing for large-scale audits  
- TypeScript SDK / browser WASM module  
- CLI tooling  
- Public Playground API deployment

---

**This API Specification is a core building block of the Audit-by-Design architecture.**  
It creates a stable, transparent, and sovereign interface that enables digital systems to be validated, tested, and audited from the very first requirement.
