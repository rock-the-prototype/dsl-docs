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
