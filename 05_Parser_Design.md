# 5. Parser Design

The parser is the component that transforms a normalized DSL statement into a structured Requirement Atom, according to the grammar defined in
02_DSL_Grammar.md and validated through the canonical JSON Schema.

t is implemented in:
📂 dsl-core/src/parser/parser.ts

## 5.1 Purpose of the Parser

The parser performs three essential tasks:

*1. Interpret normalized text*
The parser receives input that has already been normalized (canonical spacing, casing, punctuation).

*2. Extract atomic requirement components*
→ actor, modality, action, condition, result

*3. Enforce syntactic correctness*
Only valid DSL statements may be converted into Requirement Atoms.

By enforcing structure and rejecting malformed input, the parser ensures deterministic, audit-ready behavior.

## 5.2 Interaction with the Normalizer

The parser always operates on normalized input.

Processing pipeline:

Raw Input
   ↓
Normalizer (normalizer.ts)
   ↓
Parser (parser.ts)
   ↓
Requirement Atom (JSON)

Normalization ensures:

predictable capitalization of reserved keywords

canonical whitespace

a single terminator

consistent comma usage

This prevents syntactic ambiguity and improves human readability and machine reliability.

## 5.3 Grammar-to-Code Mapping

The parser implements the core structure of the DSL grammar:

As <actor>, I <modality> <action> 
    [when <condition>] 
    [then <result>].

Mapping to JSON fields:

| Grammar Element    | Parsed As   |
| ------------------ | ----------- |
| `As <actor>`       | `actor`     |
| `I <modality>`     | `modality`  |
| `<action>`         | `action`    |
| `when <condition>` | `condition` |
| `then <result>`    | `result`    |

Only binary modalities (must, must not) are accepted, ensuring determinism and RFC 2119 alignment.

## 5.4 Error Handling

The parser rejects malformed inputs and throws descriptive errors when:

- the actor clause is missing
- the modality is missing or invalid
- the action cannot be extracted
- the sentence does not match the required structure
- required keywords (As, I, must, must not) are missing

Rejecting invalid statements early is essential for:

- correctness
- safety
- traceability
- audit integrity

This strict behavior is a cornerstone of the Audit-by-Design approach.

## 5.5 Output Structure

A valid DSL statement is transformed into a *Requirement Atom*:

{
  "actor": "system",
  "modality": "must",
  "action": "validate the access token",
  "condition": "receiving an ePrescription request",
  "result": "log success or failure"
}

This output is then validated against:
📂 src/schema/requirement.schema.json

## 5.6 MVP Limitations

The current parser implementation (MVP) intentionally restricts complexity:

- one sentence per requirement
- one optional when clause
- one optional then clause
- no compound conditions (and, or)
- no nested semantics
- no multi-line constructs

These constraints ensure predictable, reproducible, and auditable behavior.

## 5.7 Roadmap for Parser Evolution

- Future enhancements include:
- support for multi-line DSL requirements
- richer error types (e.g., SyntaxError, ModalityError, ConditionError)
- compound conditions (AND / OR logic)
- Abstract Syntax Tree (AST) for deeper rule evaluation
- semantic cross-checking against governance rules
- enhanced error messages for audit logs

## 5.8 Artefact References

**Parser implementation**
📂 src/parser/parser.ts

**Normalizer**
📂 src/parser/normalizer.ts

**Canonical JSON Schema**
📂 src/schema/requirement.schema.json
