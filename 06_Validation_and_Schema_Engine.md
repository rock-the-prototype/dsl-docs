# 6. Schema Validation Engine

The Schema Validator verifies that a parsed Requirement Atom complies with the
canonical JSON Schema defined in:

📂 `dsl-core/src/schema/requirement.schema.json`

Validation uses the following deterministic pipeline:
DSL Input → Normalizer → Parser → Requirement Atom → JSON Schema Validator


## Purpose

- Ensure semantic correctness  
- Enforce strict field structure  
- Allow no extra properties  
- Enforce binary modality (`must` / `must not`)  
- Produce reproducible audit outcomes  

## Validator Output

A successful validation returns:

```json
{ "valid": true }
```

A failed validation returns:

```json
{
  "valid": false,
  "errors": [
    { "message": "...", "keyword": "enum", "path": "modality" }
  ]
}
```

---

# 5. Beispielaufruf 

```ts
import { normalizeInput } from "./src/parser/normalizer.ts";
import { parseRequirement } from "./src/parser/parser.ts";
import { validateRequirement } from "./src/validator/validator.ts";

const input =
  "As a system, I must validate the access token when receiving an ePrescription request then log success or failure.";

const normalized = normalizeInput(input);
const atom = parseRequirement(normalized);
const result = validateRequirement(atom);

console.log("Requirement Atom:", atom);
console.log("Validation Result:", result);
```
Ergebnis:

```json
Requirement Atom: {
  actor: "system",
  modality: "must",
  action: "validate the access token",
  condition: "receiving an ePrescription request",
  result: "log success or failure"
}

Validation Result: { valid: true }
```
