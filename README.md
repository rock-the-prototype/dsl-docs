# dsl-docs
Documentation and rationale for the Audit-by-Design DSL Explains the principles, design decisions, and use cases of the domain-specific language for defining, validating, and auditing atomic requirements (AFOs) in regulated software systems.

We are developing a **domain-specific language (DSL)** that enables the **atomization, validation, and traceability of requirements (AFOs)** in regulated software systems and above.

Here you'll find all docs and further instruction how to find all other information necessary:
├── README.md   

This language is designed to:

- be **human- and machine-readable** (structured text or JSON),
- represent **syntactically unambiguous and verifiable requirements**,
- be **testable, versionable, and auditable** (via Git integration),
- support **automatic validation and auditing** (CI/CD),
- and **semantically represent governance rules, negative rules, and exclusions**.


##Design Principles

| No. | Principle | Description |
|:---:|:-----------|:-------------|
| 1 | **Human readability** | Text structure and indentation must be visually logical. |
| 2 | **Machine validation** | Unambiguous grammar and reserved keywords. |
| 3 | **Parser-friendly grammar** | Formally defined, context-free (CFG-based). |
| 4 | **Extensibility** | Syntax and semantics are modular and adaptable. |
| 5 | **Toolchain integration** | Integrated with Git, CI/CD, and audit pipelines. |
| 6 | **Java-free implementation** | Focus on Deno/TypeScript or Rust environments. |
| 7 | **Rule-based prohibitions** | “Must not” and “May not” constructs for governance. |
| 8 | **Central traceability** | Full audit trail and traceability of all rule changes. |
| 9 | **Security by Design** | Validation, parsing, and data handling must guarantee protection against injection, malformed input, and misuse. The DSL never executes input, never interprets code, and enforces deterministic, safe processing at all times. |

                          
# Motivation, goal setting, design principles
👉  [**01_DSL_Goals_and_Principles.md**](https://github.com/rock-the-prototype/dsl-docs/blob/main/01_DSL_Goals_and_Principles.md)  
for more details on motivation, goal setting, and design principles.  

Digital transformation in regulated environments has reached a critical point.  
Many public digitalization projects in Germany and Europe have demonstrated the limits of fragmented, incompatible, and non-auditable infrastructures — both technically and in terms of public trust and acceptance.

Following [**Auguste Kerkhoffs’ principle**](https://rock-the-prototype.com/en/cryptography/kerckhoff-principle/) — that security must rely on transparent, verifiable systems rather than obscurity — this project applies the same logic to **requirements engineering**. Software systems that govern public or regulated domains must be **traceable, testable, and independently auditable** from the first requirement onward.

**Security by Design**
The DSL enforces strict input handling and deterministic validation to prevent injection, ambiguity, or unintended code execution.
Because the DSL treats all inputs as pure data—never executable logic—it eliminates entire classes of vulnerabilities such as XSS, RCE, and parser confusion attacks.
This aligns with Auguste Kerkhoffs’ principle: security emerges from transparency and verifiability, not obscurity.

### Why a DSL for Requirements?

The current practice of describing requirements in natural language or heterogeneous formats creates ambiguity and prevents automated validation.  

A domain-specific language (DSL) addresses this by providing:

- **Formal precision** – each requirement becomes a deterministic, testable atom.  
- **Auditability by design** – syntax and semantics enforce verifiability.  
- **Human and machine readability** – bridging expert review and automation.  
- **Governance integration** – embedding rule types, constraints, and exclusions.  

Our DSL defines the atomic unit of trust:  
a requirement that can be understood, verified, versioned, and audited — without depending on specific platforms or vendor interests.

## Architecture Overview

### How validation works - 

Validation is performed in two stages:

1. **Schema validation** ensures the requirement matches the canonical structure (shape).
2. **Rule validation** applies normative constraints (semantics) to detect violations such as
   non-binding modality, missing/placeholder actors, or atomicity violations.

The JSON Schema defines **what a requirement is**.  
The rule set defines **when a requirement is considered valid**.  
The validation engine applies both deterministically and reports rule-based errors (Rule IDs).

### Public schema contracts (dsl-core)

The canonical report output is defined as a public JSON Schema contract in `dsl-core`:

- `schemas/report.schema.json`
- `schemas/SCHEMA_CONTRACTS.md` (stability + versioning rules)

Internal engine models are not contracts (`dsl-core/src/model/**`).


## A DSL for Digital Sovereignty
This DSL establishes a formally defined, machine-verifiable language to make requirements, rules, and validations transparent, auditable, and interoperable across existing digital infrastructures and usable for any sector or domain.

### Context & Trust

True digital sovereignty does not come from building yet another isolated infrastructure but from **making existing ones transparent and auditable**.  
Instead of debating shortcomings, this initiative proposes a constructive, consensus-oriented approach that:

- enables voluntary participation and self-determined data control,  
- fosters interoperability through open, standard-based semantics, and  
- ensures that every rule, validation, and exception remains inspectable.  

In essence, **trust arises from transparency**, and transparency begins with the language we use to define what systems must — and must not — do.


# Getting Started
The Audit-by-Design DSL is actually divided into two core repositories —  
one for **concept and specification**, and one for **implementation and validation**.

| Repository | Purpose | Access |
|-------------|----------|--------|
| [`rock-the-prototype/dsl-docs`](https://github.com/rock-the-prototype/dsl-docs) | Documentation, principles, grammar, and design rationale | You are here ✅ |
| [`rock-the-prototype/dsl-core`](https://github.com/rock-the-prototype/dsl-core) | Core specification and schemas for machine validation and parser integration | [Open Repository →](https://github.com/rock-the-prototype/dsl-core) |

### 1️⃣ Explore the Concept

Start here in `dsl-docs` to understand:
- the motivation and guiding principles ([01_DSL_Goals_and_Principles.md](./01_DSL_Goals_and_Principles.md)),
- the formal grammar definition ([02_DSL_Grammar.md](./02_DSL_Grammar.md)),
- and how validation and auditability are built into secure design.

These documents explain **why** the DSL exists, **how** it is structured,  
and **which principles** ensure auditability, transparency, and regulatory alignment.

### 2️⃣ Move to the Core

Once you understand the conceptual foundation,  
visit [`dsl-core`](https://github.com/rock-the-prototype/dsl-core) to see how the DSL is implemented.

There you’ll find:
- the current **JSON schemas** and **validation logic**,  
- examples of atomic requirement definitions,  
- and the integration points for **CI/CD validation pipelines**.

The `dsl-core` repository is where **machine-readable trust begins** —  
it provides the structural backbone for any parser, validator, or compliance framework built on top of this DSL.

## 🚀 Quickstart (try it in 5 minutes)

**Normalize an AFO from text**
echo "Die Software MUSS DSGVO einhalten." | deno run --no-prompt -A src/cli.ts normalize

**Validate a normalized JSON file**
deno run --no-prompt -A src/cli.ts validate ./examples/afo.json

**Validate a folder of AFOs**
find ./examples -name "*.json" | deno run --no-prompt -A src/cli.ts normalize

### 3️⃣ (Optional) Contribute & Participate

The DSL is open for participation.  
Whether you are a developer, architect, researcher, or compliance expert —  
your contributions help make requirements transparent, testable, and verifiable for everyone.

If you’d like to join, please see  
👉 [**How to Contribute**](./CONTRIBUTING.md) *(coming soon)*  
for details on collaboration, governance rules, and peer review.

---

## Why participation matters  
❇️ Audit-by-Design is not just a concept — it is an invitation to co-create a transparent, verifiable foundation for digital trust. ❇️ Every contribution strengthens the collective ability to build software that is explainable, compliant, and accountable by secure design.
❇️ 

## Organizational benefits of participation

By contributing to the Audit-by-Design DSL initiative, organizations gain tangible and strategic advantages:

- **Verifiable compliance**  
  Each requirement can be linked to audit rules and legal references (e.g. AFOs, BSI TRs, EUDI).  
  This reduces audit effort, supports conformity checks, and shortens certification cycles.

- **Traceable development processes**  
  The DSL integrates directly with Git-based workflows, 
  enabling continuous validation and historical traceability of all requirement changes.

- **Improved software assurance**  
  Formalized requirements reduce ambiguity in specifications and vendor contracts.  
  Testing and validation become reproducible and verifiable.

- **Trust and transparency by design**  
  Publicly understandable and machine-readable requirements build confidence  
  among users, partners, and oversight bodies — not just within IT,  
  but across management, governance, and compliance departments.

- **Cross-sector interoperability**  
  Shared semantics allow organizations from different sectors  
  to align their audit criteria and technical standards without revealing proprietary data.

- **Reduced operational risk**  
  Consistent definitions and automated validation help detect conflicts,  
  redundancies, and gaps early — before they manifest in production or compliance audits.

- **Sovereignty through open standards**  
  Participation ensures independence from proprietary toolchains  
  and strengthens collective control over digital trust infrastructures.

In short: **Organizations that contribute to the DSL are shaping the standard  
for trustworthy digital services — not adapting secure design to it later.**

```mermaid
flowchart TD
    A[Human DSL Input]
    B["Normalization<br/>(Canonical Form)"]
    C["Formal Grammar / Parser<br/>(Does the requirement exist?)"]
    D["Requirement Atom<br/>(Candidate)"]
    E["Rules & Validation<br/>(Is it a good requirement?)"]
    F["Audit-Ready Requirement Atom"]

    A --> B
    B --> C
    C -->|valid only| D
    D --> E
    E -->|passes all rules| F
```    
  
Defines the smallest atomic unit of the Audit-by-Design DSL —  
a **Requirement Atom** — based on a deterministic, context-free grammar (CFG).

## Formal Grammar, Syntax Principles & Examples

The Audit-by-Design DSL uses a **context-free grammar (CFG)** to describe  
the smallest verifiable unit of a requirement — the **Requirement Atom**.  
This grammar defines how human-readable statements can be parsed into  
machine-interpretable structures for validation, testing, and audit reporting.

### Grammar Definition (EBNF)

```ebnf
<requirement> ::= <sentence> <terminator>

<sentence> ::= <actor_clause> <action_clause> [<condition_clause>] [<result_clause>]

<actor_clause> ::= "As" <role>
<action_clause> ::= "I" <modality> <action>
<condition_clause> ::= "when" <condition>
<result_clause> ::= "then" <expected_result>

<terminator> ::= "."

<modality> ::= "must" | "must not" 

<role> ::= <identifier>
<action> ::= <verb_phrase>
<condition> ::= <clause>
<expected_result> ::= <clause>

<identifier> ::= [A-Za-z0-9_-]+
<verb_phrase> ::= [A-Za-z ]+
<clause> ::= [A-Za-z0-9 ,_()-]+
```

*“As a system,  
I must validate the access token  
when receiving an ePrescription request  
then log success or failure.”*

### Why this PROOF of IT SECURITY matters

In regulated environments, every software action that processes, stores,  
or transmits data must be **traceable, verifiable, and reproducible**.  
That is the essence of *Secure by Design* and *Audit by Design*.  

The Audit-by-Design DSL ensures that such requirements are written  
in a form that is **unambiguous, testable, and machine-validated**.  
Even a simple statement like the following demonstrates how  
security, compliance, and auditability are expressed in one atomic sentence.

### Modalities and Determinism

The Audit-by-Design DSL restricts modalities to a **binary logic**  
in order to maintain full determinism and auditability.

*Rationale*

Only binary modalities can be deterministically evaluated — either a condition is fulfilled or it is violated.
Anything else (e.g., “should” or “may”) would introduce uncertainty and therefore be non-deterministic, making it unsuitable for CI/CD validation and automated compliance checks.

This strict modality model ensures traceability and verification in continuous audit pipelines, where every rule must yield a reproducible result: pass or fail.

Alignment with RFC 2119

This decision aligns with the IETF RFC 2119

standard for normative language in technical specifications,
which defines:

MUST / MUST NOT – absolute requirements of the specification

SHOULD / MAY – optional or recommended behavior, not mandatory

By limiting the DSL to “must” and “must not”,
we preserve semantic precision, regulatory coherence,
and machine verifiability — all essential pillars of SECURE Audit-by-Design.


│
│         
# Parser/Validator, audit concept
│
├── 03_Validation_and_Audit.md  
├── 04_Examples.md    
│
│
# DSL-Examples (Text + JSON)
├── 05_Governance_and_Rules.md    
│
│
│
# Change processes, compliance, versioning
└── assets/
    ├── diagrams/
    │   └── dsl-cfg-structure.png         
    # later CFG graphic
    └── examples/
        └── requirement_atom.json         
        # JSON example for reference


## License

The **Audit-by-Design DSL Docs ** are released under  
[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).  
© 2025 Sascha Block / Rock the Prototype

The **Audit-by-Design DSL Core ** are under Copyright 2025 Sascha Block and are licensed under the Apache License, Version 2.0 (the "License"); you may not use the files except in compliance with the License.
You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
 
