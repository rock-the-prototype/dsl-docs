# 7. Superior IT-Security Design Principles  
*Security by Design for the Audit-by-Design DSL*

The Audit-by-Design DSL is intentionally engineered with a strict security model.  
It treats all requirement statements as pure data — never executable logic — and therefore eliminates entire classes of vulnerabilities commonly found in dynamic or loosely defined languages.

This document defines the security principles, threat models, and protective design decisions that govern the DSL and the surrounding tooling (parser, validator, playground, CI/CD integration).

---

## 1. Security by Design — Core Principle

Security must never be an afterthought anytime, it is foundational.  
Following **Auguste Kerckhoffs’ principle** — that security must rely on transparent, verifiable systems rather than obscurity — the DSL enforces:

- deterministic parsing,  
- strict grammar,  
- canonical normalization,  
- schema validation,  
- and complete avoidance of dynamic execution paths.

The DSL treats all input exclusively as structured data — never as executable logic.
This is a security measure as well as our fundamental design decision:
requirements in software, especially in regulated environments must be human-readable and machine-verifiable.

Executable code is inherently ambiguous, unsafe, and needs addional attention in validation and audits.
Regulatory-grade requirements, however, must exist in a canonical, contradiction-free, deterministic normal form.
The DSL provides exactly that:
a language that preserves human clarity while enabling machine validation — without ever allowing code-like constructs.

---

## 2. Threat Model Overview

The following attack classes were assessed as high-risk for any system accepting free-text input:

| Threat Category | Description | Severity | DSL Mitigation |
|-----------------|-------------|----------|----------------|
| **XSS (Cross-Site Scripting)** | Injection of HTML/JS via input fields | High | DSL treats input as plain text; no HTML reflection; no browser execution |
| **RCE (Remote Code Execution)** | Input interpreted as executable code | **Critical** | No eval, no dynamic execution, strict grammar, pure string parsing |
| **Injection Attacks (General)** | Attempt to break out of grammar into commands | High | CFG-based parser rejects all non-grammar input |
| **Parser Confusion Attacks** | Ambiguity leading to mis-interpreted rules | High | Deterministic grammar + Schema validation |
| **Malformed Input** | Partial sentences, missing segments | Medium | Hard rejection; strict error handling |
| **Denial of Service via malformed input** | Heavy recursion, deep branches | Medium | Non-recursive top-down parsing; fixed grammar depth |

The DSL’s design ensures that none of these attacks can escalate into code execution or privilege escalation.

---

## 3. Protective Design Decisions

### 3.1 No Code Execution — Ever  
- No template engines  
- No evaluation of expressions  
- No dynamic function dispatch  
- No plugin injection  
- All inputs treated as immutable strings

→ **Remote Code Execution becomes categorically impossible.**

---

### 3.2 Deterministic, Context-Free Grammar  
The DSL relies on a **context-free grammar (CFG)**.  
This provides:

- strict boundaries for allowed constructs  
- unambiguous parse trees  
- no interpretation outside grammar  
- predictable error behavior  

→ Prevents parser confusion attacks.

---

### 3.3 Canonical Normalization  
All DSL input undergoes normalization:

- whitespace normalization  
- canonical casing of keywords  
- enforced terminators  
- removal of ambiguous punctuation  

This prevents:

- hidden control characters  
- Unicode tricks  
- bypassing parser rules  
- ambiguous grammar branches

See:  
`dsl-core/src/parser/normalizer.ts`

---

### 3.4 Strict JSON Schema Validation  
After parsing, all Requirement Atoms must comply with the schema:  
`dsl-core/src/schema/requirement.schema.json`

Key protections:

- `enum` for modalities → prevents semantic injection  
- `additionalProperties: false` → no hidden fields  
- required fields enforce atomic consistency  
- everything becomes machine-verifiable

---

### 3.5 Pure Functions, No Side Effects  
All parsing and validation functions are pure:

- no I/O  
- no filesystem access  
- no environment variable injection  
- no external calls  

This is essential for **auditability** and **sandboxing**.

---

### 3.6 Input Isolation in the Playground  
The DSL Playground server enforces:

- no HTML rendering of user input  
- no script injection  
- no DOM interaction  
- pure text output in `<pre>` blocks  
- all content treated as literal text  

→ **XSS cannot occur**, even intentionally.

---

## 4. Security Controls (Mandatory)

These controls are part of the DSL’s long-term stability strategy:

### Control A — Reject All Unknown Constructs  
Any deviation from the defined grammar must throw a hard error.

### Control B — Input Is Immutable Data  
No conversion into HTML, JS, templates, shell commands, or system calls.

### Control C — Deterministic Fail-Fast Design  
First failure stops execution; no partial or silent errors.

### Control D — No Inline Interpretation  
The DSL core must never interpret:

- regex from user input  
- dynamic callbacks  
- function references  
- interpreted expressions  

### Control E — Deployments Must Be Sandboxed  
Deno already ensures:

- no network access unless allowed  
- no filesystem access unless allowed  
- no subprocess execution  

The Playground uses **zero additional permissions**.

---

## 5. Security Guarantees

Because of its design, the DSL provides several guarantees:

| Guarantee | Meaning |
|----------|---------|
| **Non-Executable Language** | No input can produce code execution |
| **Deterministic Parsing** | Same input → same output → audit stability |
| **Strict Error Surfaces** | Errors are explicit, never hidden |
| **Isolation From Browser Runtime** | No HTML injection or script execution |
| **Immutable Requirement Atoms** | Atomic structure enforced by schema |
| **Predictable CI/CD Validation** | No nondeterministic behavior |

---

## 6. Why This Matters for Regulated Domains

Sectors such as healthcare, energy, administration, telecommunications, and payment require:

- Traceable requirements
- Auditable processes
- Deterministic machine validation
- Audit-proof pipelines  

The slightest semantic or syntactic error can:

- jeopardize certifications,  
- cause audits to fail,  
- enable security exploits,  
- trigger regulatory liability.

→ With this DSL, security is anchored **at the requirement level** — not just in the code.


---

## 7. Roadmap — Future Enhancements

- Formal threat model document (MITRE ATT&CK + STRIDE)
- Integration of static analysis for requirement sets
- Detection of contradictory requirements
- Governance rules for secure requirement evolution
- Policy-based validation for high-risk domains (eHealth, Energy, GovTech)

---

## 8. Summary

The Superior IT-Security Design Principles define why the Audit-by-Design DSL is inherently safe:

- **No execution paths**  
- **Strict grammar**  
- **Canonical normalization**  
- **Schema validation**  
- **Sandboxing**  
- **Complete transparency**  

The DSL treats all input exclusively as structured data — never as executable logic.
This is both a security measure and a fundamental design decision:
requirements in software systems, especially in regulated environments, must be human-readable and machine-verifiable.

Executable code is inherently ambiguous, unsafe, and requires additional layers of validation, review, and audit controls.
Regulatory-grade requirements, however, must exist in a canonical, contradiction-free, deterministic normal form.
The DSL provides exactly that:
a language that preserves human clarity while enabling strict machine validation — without ever allowing code-like constructs.

This definition establishes the foundation for building secure, traceable, and sovereign digital infrastructures with the DSL at their core — fully aligned with Auguste Kerckhoffs’ principle that trustworthy systems must rely on transparency and verifiability, not obscurity.
Scientifically legitimized — and with best regards from Auguste Kerckhoffs.
