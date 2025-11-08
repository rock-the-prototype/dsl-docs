# 3. Formal Grammar (CFG – Context-Free Grammar)

The grammar defines the smallest atomic unit of our DSL — a **Requirement Atom**.  
This CFG is technology-agnostic and can be implemented in **Deno**, **Rust**, or **Python**.

```ebnf
<requirement> ::= <sentence> <terminator>

<sentence> ::= <actor_clause> <action_clause> [<condition_clause>] [<result_clause>]

<actor_clause> ::= "As" <role>
<action_clause> ::= "I" <modality> <action>
<condition_clause> ::= "when" <condition>
<result_clause> ::= "then" <expected_result>

<modality> ::= "must" | "must not"

```

### Reference and Origin

This grammar conceptually extends the *User Story* structure introduced by  
**Mike Cohn (2004, *User Stories Applied*, Addison-Wesley)**,  
as further discussed in *Large-Scale Agile Frameworks* by **Sascha Block (Springer-
Verlag GmbH Deutschland, Part of Springer Nature 2023)**.  

> *“User stories consist of three essential elements:  
> the written part, the conversation around the story, and the acceptance criteria.”*  
> — Sascha Block, *Large-Scale Agile Frameworks*, Chapter 7.2.2 “User Story”,  
> Springer, ISBN 978-3-662-67781-0 / eBook ISBN 978-3-662-67782-7  
> [https://doi.org/10.1007/978-3-662-67782-7](https://doi.org/10.1007/978-3-662-67782-7)

While the original Cohn format (`As a <user>, I want <goal>, so that <reason>`) focuses on  
communication and shared understanding within agile teams,  
the **Audit-by-Design DSL** extends this principle into a **formal, machine-readable grammar**  
for expressing **deterministic, auditable, and legally compliant requirements** in regulated domains.

*Rationale*

Only binary modalities can be deterministically evaluated —
either a condition is fulfilled or it is violated.
Anything else (e.g., “should” or “may”) would introduce uncertainty
and therefore be non-deterministic, making it unsuitable
for CI/CD validation and automated compliance checks.

This strict modality model ensures traceability and verification
in continuous audit pipelines, where every rule must yield
a reproducible result: pass or fail.

Alignment with RFC 2119 – Key words for use in RFCs to Indicate Requirement Levels

This decision aligns with the Internet Engineering Task Force (IETF)
standard RFC 2119
,
which defines the normative meaning of modal verbs in technical specifications.

MUST / MUST NOT – absolute requirements of the specification

SHOULD / MAY – optional or recommended behavior, not mandatory

By limiting the DSL to “must” and “must not”,
we preserve semantic precision, regulatory coherence,
and machine verifiability — all essential pillars of Audit-by-Design.

### Syntax Principles

| Principle | Example | Explanation |
|------------|----------|-------------|
| **A sentence is the smallest unit.** | *“As a system, I must validate access tokens.”* | Each requirement is a complete statement ending with a period (`.`). |
| **Stop commands end a unit.** | `.` = logical termination | The parser identifies the sentence boundary deterministically. |
| **Indentation is semantically unique.** | `2 or 4 spaces → sub-context` | Enables both human readability and structural parsing. |
| **Basic Commands = Keywords.** | `As`, `I must`, `when`, `then` | Reserved tokens with fixed meaning in grammar. |
| **Governance logic through modality.** | `must`, `must not` | Defines mandatory or prohibited behavior. |


## 2️⃣ Example Section 
Examples are essential to illustrate how the formal grammar translates into practice.  
They show that the DSL is not just theoretically consistent, but directly applicable —  
clear enough for humans to understand, and precise enough for machines to verify.

### Requirement Atom

*“As a system,  
I must validate the access token  
when receiving an ePrescription request  
then log success or failure.”*

This sentence expresses a complete requirement in plain, unambiguous language.  
It defines **who** acts (the system - without use of further component definition), **what** must happen (validate the access token),  
**under which condition** (when receiving an ePrescription request),  
and **how accountability is ensured** (by logging success or failure).  

It’s written this way because every clause carries a distinct, testable meaning —  
together forming one atomic, auditable unit of trust.

#### JSON Representation

This example demonstrates how the same requirement can be represented
in both human-readable and machine-readable forms,
without losing semantic meaning or auditability.

```json
{
  "id": "A_12302",
  "actor": "system",
  "modality": "must",
  "action": "validate the access token",
  "condition": "when receiving an ePrescription request",
  "result": "then log success or failure"
}
```
## Why Context-Free Grammars (CFG) Matter

To design a domain-specific language (DSL) that is **auditable, machine-readable, and automatically verifiable**, we need a formal foundation that defines how valid sentences are built and interpreted.

That foundation is provided by **context-free grammars (CFGs)** —  
the mathematical model behind most modern programming and specification languages.

### Why CFGs are essential for our DSL

- **Well-structured syntax**  
  CFGs describe hierarchical structures through production rules,  
  ensuring that every statement in the DSL can be parsed and represented as a syntax tree.

- **Deterministic parsing**  
  Each rule in a CFG defines exactly how a sentence can be decomposed,  
  allowing machines to validate syntax and semantics unambiguously.

- **Automatic validation**  
  A formally defined grammar enables continuous testing, linting,  
  and compliance validation within CI/CD pipelines — without human interpretation.

- **Compatibility with structured data**  
  CFGs align perfectly with JSON- and YAML-like representations,  
  making them ideal for both textual and machine-readable DSLs.

### Summary

Developing our own DSL means defining a **well-structured, unambiguous, and parsable language**.  
This is only possible with a **formal grammar** — and that grammar must be **context-free**, so that every requirement can be analyzed, validated, and audited without contextual ambiguity.
