# 4. Validation & Tests

This section documents how DSL statements are validated, parsed, and tested inside the dsl-core runtime.

## 4.1 Validation Layer

Validation is performed in three deterministic layers:

1️⃣ Normalization
Transforms any user input into a canonical form by fixing spacing, casing, commas, and terminators.

2️⃣ Parsing (CFG-based)
Ensures syntactic correctness according to the formal DSL grammar (defined in 02_DSL_Grammar.md).

3️⃣ Schema Validation (JSON Schema)
Ensures semantic correctness of Requirement Atoms using the canonical JSON Schema definition.

Together, these layers guarantee deterministic, reproducible auditability.

## 4.2 Validation Pipeline Overview

User Input
   ↓
Normalization
   ↓
Parser (CFG rules)
   ↓
Requirement Atom (JSON object)
   ↓
JSON Schema Validation
   ↓
Audit / CI/CD Pipeline

*Why this matters*

- Formal validation prevents:
- ambiguous requirements
- inconsistent formatting
- partial or incomplete statements
- silently accepted errors

Every requirement becomes machine-verifiable and human-readable, a fundamental principle of Audit-by-Design.

## 4.3 Example Test Cases

The following examples illustrate how the DSL parser behaves for valid and invalid inputs.

All tests are located in:
dsl-core/tests/parser.test.ts

## 4.3.1 Positive Test — Valid Requirement Atom

Deno.test("Parsing a valid DSL Requirement Atom", () => {
  const input =
    "As a system, I must validate the access token when receiving an ePrescription request then log success or failure.";

  const result = parseRequirement(input);

  assertEquals(result.actor, "system");
  assertEquals(result.modality, "must");
  assertEquals(result.action, "validate the access token");
});

A valid Requirement Atom must include:

- actor
- binary modality (must / must not)
- action
- optional: condition + result

These elements map directly to the formal grammar in
 [`rock-the-prototype/dsl-docs/02_DSL_Grammar.md`](https://github.com/rock-the-prototype/dsl-docs/blob/main/02_DSL_Grammar.md)  

### 4.3.2 Normalization Test — Forgiving Input, Strict Output

Deno.test("Normalizes inconsistent spacing and casing", () => {
  const input =
    "  as  a SYSTEM ,   i   MUST   validate the Access Token   WHEN receiving data THEN   log Success   or Failure   . ";

  const result = parseRequirement(input);

  assertEquals(result.actor, "system");
  assertEquals(result.modality, "must");
});

Normalization ensures:

- canonical spacing
- canonical casing
- canonical punctuation

This produces deterministic results in CI/CD pipelines and audit processes.

### 4.3.3 Negative Tests — Rejecting Invalid Syntax

Invalid statements must never be parsed into Requirement Atoms.

Deno.test("Rejects invalid syntax (missing modality)", () => {
  const invalid = "As a system, I validate the access token.";
  assertThrows(() => parseRequirement(invalid), Error);
});

Deno.test("Rejects invalid syntax (missing actor)", () => {
  const invalid = "I must validate the access token.";
  assertThrows(() => parseRequirement(invalid), Error);
});

Deno.test("Rejects empty input", () => {
  assertThrows(() => parseRequirement(""), Error);
});

Rejecting malformed input is essential for:

- safety
- correctness
- audit integrity
- avoiding silent failure

## 4.4 Compliance with JSON Schema

After parsing, the Requirement Atom is validated against the canonical JSON Schema:

dsl-core/src/schema/requirement.schema.json 

JSON Schema ensures:

- strict field structure
- required fields enforced
- binary modality only
- no additional properties allowed

This closes the gap between syntax and semantics.

## 4.5 Validation Result 
The DSL validation strategy ensures that every requirement is:

- normalized
- syntactically correct
- semantically valid
- schema-compliant
- deterministically testable
- audit-ready

This validation pipeline is a core pillar of the Audit-by-Design philosophy and ensures that all requirements behave predictably across implementations, environments, and organizations.
