# 3. Normalization and Canonical Form

The normalization process ensures that every DSL statement — regardless of formatting, spacing, or capitalization —  
can be transformed into a **Canonical Form** for deterministic validation and audit traceability.

## Artefact Type and Function 

> **Artefact:** [`src/parser/normalizer.ts`](https://github.com/rock-the-prototype/dsl-core/blob/main/src/parser/normalizer.ts)  
> **Type:** Functional implementation (Normalization Logic)  
> **Responsibility:** Converts free-form human input into a canonical, audit-ready representation of a DSL statement.  
> **Version Control:** All modifications to this artefact are versioned and traceable through Git commit history and semantic versioning.

## Purpose

Normalization is a pre-processing step that:
- removes redundant whitespace and punctuation inconsistencies,
- harmonizes keyword casing (`As`, `I`, `When`, `Then`),
- ensures each statement ends with exactly one period (`.`),
- and prepares the text for parser-level tokenization.

## Canonical Output Rules

Normalization guarantees that the parser always receives a predictable and machine-verifiable input form. A normalized DSL statement MUST satisfy the following canonical constraints:

1. Lowercase semantics
All semantic tokens (actor, modality, action, condition, result) are normalized to lowercase. This ensures deterministic comparison and hashing.

2. Whitespace normalization
- multiple spaces → single space
- no leading/trailing whitespace
- standardized comma spacing: ", "

3. Deterministic keyword casing
DSL keywords (As, I, When, Then) retain consistent capitalisation for readability but the parsed fields are always lowercase.

4. Single terminating period
Every DSL statement MUST end with exactly one ".".

5. Normalization as an audit requirement
Canonical Form is the basis for generating hash-stable audit artefacts
→ different user input, same canonical output → same audit trail.

## Implementation Reference

> The normalization logic is implemented next to the parser module in:  
> [`rock-the-prototype/dsl-core/src/parser/normalizer.ts`](https://github.com/rock-the-prototype/dsl-core/blob/main/src/parser/normalizer.ts)  
> and converts free-form text into a canonical, audit-ready statement.

```ts
export function normalizeInput(input: string): string {
  return input
    .replace(/\s+/g, " ")
    .replace(/\s*,\s*/g, ", ")
    .replace(/\s*\.\s*$/, ".")
    .replace(/\b(as|i|when|then|must|must not)\b/gi, (m) =>
      m.charAt(0).toUpperCase() + m.slice(1).toLowerCase()
    )
    .trim();
}
```
```ts
export function normalizeInput(input: string): string {
  ...
}
```

## Security-First Normalization

To protect downstream systems that rely on automated validation, the normalization process also removes potentially harmful input patterns:

- HTML tags (<script>, <img>, <svg onload> …)
- invisible Unicode separators (e.g., U+200B, U+00A0)
- homoglyph attack characters (e.g., Cyrillic а vs Latin a)
- control characters (\u0000–\u001F)

These transformations ensure that the DSL can be safely used in:
- CI/CD pipelines
- automated compliance scanners
- audit trails
- security-critical systems (KRITIS, healthcare especially german telematic infrastructure, finance)

### JSON Schema Reference

The canonical machine-readable definition of a Requirement Atom is provided as a JSON Schema, located in  
[`dsl-core/src/schema/requirement.schema.json`](https://github.com/rock-the-prototype/dsl-core/blob/main/src/schema/requirement.schema.json).

This schema follows the [JSON Schema Draft-07 Specification](https://json-schema.org/draft-07/json-schema-release-notes.html), ensuring compatibility across a wide range of validation libraries and CI/CD pipelines.

#### Why JSON Schema?

JSON Schema provides a formal contract that:
- defines the structure and allowed values of each requirement,
- enables automatic validation of artifacts,
- and ensures reproducibility across implementations.

## Canonical Contract Between Parser, Validator, and External Systems

The JSON Schema defines the canonical machine contract between:
- the DSL parser (producer),
- the DSL validator (consumer),
- audit engines,
- external governance tools,
- CI/CD automation,
- and future commercial validator products.

All normalized RequirementAtoms MUST conform exactly to this schema.
This guarantees:
- deterministic validation
- platform-independent behavior
- reproducible audit results
- stable hashing for audit chains
- forward compatibility across DSL versions

The canonical contract is the foundation that allows different runtime environments
(Deno, Python, Rust, Java, Go) to evaluate the same DSL requirement identically.

```ts
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "RequirementAtom",
  "description": "Canonical definition of an atomic requirement (AFO) in the Audit-by-Design DSL.",
  "type": "object",

  // Canonical fields of a normalized RequirementAtom
  "properties": {
    "actor": {
      "type": "string",
      "description": "Entity performing the action, normalized to lowercase and trimmed.",
      "minLength": 2,
      "maxLength": 50,
      "pattern": "^[A-Za-z0-9 _-]+$"
    },

    "modality": {
      "type": "string",
      "enum": ["must", "must not"],
      "description": "Normative strength as defined by RFC 2119."
    },

    "action": {
      "type": "string",
      "description": "Describes what the actor must do.",
      "minLength": 3,
      "maxLength": 200,
      "pattern": "^[A-Za-z0-9 ,._-]+$"
    },

    "condition": {
      "type": "string",
      "description": "Optional condition under which the action applies.",
      "minLength": 3,
      "maxLength": 200,
      "pattern": "^[A-Za-z0-9 ,._-]+$"
    },

    "result": {
      "type": "string",
      "description": "Expected outcome of the action.",
      "minLength": 3,
      "maxLength": 200,
      "pattern": "^[A-Za-z0-9 ,._-]+$"
    }
  },

  // Canonical minimal requirement: actor + modality + action
  "required": ["actor", "modality", "action"],

  // Prevent user-defined or accidental fields
  "additionalProperties": false,

  // Examples for documentation/user tooling
  "examples": [
    {
      "actor": "system",
      "modality": "must",
      "action": "validate the access token",
      "condition": "upon receiving a request",
      "result": "log success or failure"
    }
  ]
}
```

#### Key Design Decisions

- **Draft-07** was chosen for maximum tool compatibility and long-term stability.  
- **Minimal required fields** (`actor`, `modality`, `action`) enforce atomic consistency.  
- **Additional properties disabled** (`additionalProperties: false`) ensures strict typing.  
- **Binary modality (`must` / `must not`)** preserves deterministic validation.

## Atomicity Rules

The JSON Schema enforces atomicity indirectly:
- exactly one action
- at most one condition
- exactly one result
- no additional properties

Combined with the parser's THEN segmentation logic, this ensures:
✔ One requirement = one semantic obligation
✔ No compound chains like “… then do X and then do Y”
✔ Perfect traceability for audit logs and compliance engines

Atomicity is essential for:
- deterministic validation,
- unambiguous compliance scoring,
- reproducible audit trails,
- and NIS2-aligned governance reporting.

By aligning with JSON Schema, the DSL can be validated in any runtime (Deno, Rust, Python) with identical results — a cornerstone of *Audit-by-Design*.
