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
