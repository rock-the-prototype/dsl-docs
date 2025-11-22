# 6. DSL Playground & MVP

The DSL Playground is the first user-facing implementation of the Audit-by-Design DSL, enabling anyone to experiment with the language directly in the browser — without installing any tooling.

It serves as the Minimum Viable Product (MVP) of the DSL ecosystem and demonstrates the complete validation pipeline in a simple, transparent way.

## 6.1 Purpose of the Playground

The Playground enables:

- developers
- software and IT architects
- auditors
- analysts
- product owners
- students and researchers

to explore the DSL with instant feedback.

It exploratively improves:

- discoverability
- onboarding
- teachability
- transparency
- trust

and allows stakeholders to understand why deterministic, auditable requirements matter.

## 6.2 Key Features

- **Minimal Deno HTTP** server using std/http (zero dependencies)
- **Clean HTML interface** for entering DSL statements
- **Real-time output** showing:
  a) normalized form
  b) parsed Requirement Atom
  c) semantic validation against JSON Schema
- **Structured JSON feedback** for auditability
- **Clear error messages** with human-readable explanations

The server runs locally and remains fully compliant with the project's digital sovereignty philosophy — no third-party services, no external resources, no telemetry.

## 6.3 What the Playground Demonstrates

The Playground visualizes the entire pipeline:

User Input
   ↓
Normalization
   ↓
Parsing (CFG)
   ↓
Requirement Atom (JSON)
   ↓
JSON Schema Validation
   ↓
Outcome (valid or error)

It shows:

1) how input is normalized deterministically
2) how the parser extracts grammar-compliant Requirement Atoms
3) how semantic validation enforces structure and modality
4) how errors surface in an audit-friendly manner
5) how DSL-core will integrate into future tooling

This makes the DSL principles tangible.

## 6.4 Motivation and Design Rationale

This Playground represents the first functional integration point for the entire DSL ecosystem.

It is intentionally:

- simple
- dependency-free
- sovereign
- extensible
- understandable

…to form a stable baseline for upcoming tooling.

It lays the groundwork for:

- a rich web editor (syntax highlighting, AST view)
- a local CLI
- CI/CD audit pipelines
- an online demo instance
- governance rule simulators

The Playground is therefore not the bridge that connects theory (DSL docs) and practice (dsl-core).

## 6.5 Source Code Reference

The Playground lives in the main DSL runtime:

📁 dsl-core/server.ts

It uses:
- normalizeInput
- parseRequirement
- validateRequirementAtom

from dsl-core to deliver a fully functional audit loop.
