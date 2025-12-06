# 6. DSL Playground & MVP

The DSL Playground and the CLI together represent the first user-facing implementations of the Audit-by-Design DSL.
They make the entire validation pipeline directly observable, transparent, and testable — either in the browser or via command line.

Both serve as foundational components for the Minimum Viable Product (MVP) of the DSL ecosystem.

## 6.1 Purpose of the Playground

The Playground enables:

- developers
- software and IT architects
- auditors
- analysts
- product owners
- students and researchers

to explore the DSL with instant feedback.

It improves:

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

## 6.3 Command Line Interface (CLI)

Beyond the browser-based Playground, the DSL now includes a standalone CLI tool:
```console
deno run --allow-read --allow-net --no-prompt src/cli.ts normalize
```

The CLI enables:
- interactive user input
- canonical normalization
- deterministic parsing into a Requirement Atom
- structural validation via JSON Schema
- atomicity checks
- reproducible JSON output compatible with CI/CD

CLI Output Pipeline
1. Raw Input
2. Canonical Normalization
3. Parsed Requirement Atom (JSON)
4. Validation Report (success or detailed error taxonomy)

The CLI forms the technical foundation for:
- automated audit pipelines
- GitHub Actions
- REST /validate endpoints
- enterprise governance tooling


## 6.4 What the Playground and CLI Demonstrate

Both implementations visualize the full DSL audit pipeline:

User Input
   ↓
Normalization (canonical form)
   ↓
Parsing (CFG, grammar extraction)
   ↓
Requirement Atom (JSON)
   ↓
JSON Schema Validation
   ↓
Outcome (valid or error)

They demonstrate:

1) how input is normalized deterministically
2) how the parser extracts grammar-compliant Requirement Atoms
3) how semantic validation enforces structure and modality
4) how errors surface in an audit-friendly manner
5) how DSL-core will integrate into future tooling

This makes the DSL principles of Audit-by-Design tangible and verifiable.

## 6.5 JSON Schema as Canonical Contract

The Browser Playground and CLI both rely on the canonical JSON Schema located at:
📁 dsl-core/src/schema/requirement.schema.json

This schema defines the machine-verifiable structure of every Requirement Atom:
- strict field structure
- allowed character sets
- length constraints
- modality according to RFC 2119
- canonical atomicity boundaries
- additionalProperties: false

Any normalized DSL statement MUST conform to this schema to be considered valid.

Because the schema uses JSON Schema Draft-07, validation behaves consistently across runtimes such as:
- Deno
- Node.js
- Python
- Rust
- Go
- Java

This guarantees interoperability and audit traceability across different implementations.

## 6.6 Atomicity Enforcement

The DSL enforces that every requirement expresses exactly one atomic requirement (AFO).

This means:
- one actor
- one modality
- one action
- at most one condition
- one result
- no chained THEN clauses
- no compound outcomes such as “log success and notify audit service”

Atomicity is required for:

- deterministic validation
- risk classification
- governance rule mapping
- audit scoring
- reproducible compliance evaluation

The parser and validator cooperate to reject any requirement that expresses more than one atomic behavior.


## 6.7 Motivation and Design Rationale

The Playground and CLI represent the first functional integration point for the entire DSL ecosystem.

It is intentionally:

- are intentionally simple
- have zero external dependencies = dependency-free
- operate fully locally and are designed for decentralized networks
- follow a digital sovereignty principle
- expose the complete validation pipeline
- provide an easy to understand and transparent baseline for research, audits, and product teams

They collectively form the first functional integration point of the DSL ecosystem.

They also lay the foundation for:

- a rich web editor (syntax highlighting, AST visualization)
- a dedicated local CLI for enterprise deployments
- CI/CD audit pipelines (GitHub Actions, GitLab CI, Tekton)
- REST-based validation APIs
- governance rule simulators
- enterprise compliance engines

The Playground is therefore not the bridge that connects theory (DSL docs) and practice (dsl-core).

## 6.8 Source Code Reference

The Playground and CLI both use the following DSL-core components:

📁 dsl-core/src/parser/normalizer.ts — canonical normalization
📁 dsl-core/src/parser/parser.ts — deterministic grammar extraction
📁 dsl-core/src/validator/validator.ts — structural validation + atomicity rules
📁 dsl-core/src/schema/requirement.schema.json — canonical JSON Schema contract

