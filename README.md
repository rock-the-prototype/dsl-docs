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

                          
# Motivation, goal setting, design principles
👉  [**01_DSL_Goals_and_Principles.md**](https://github.com/rock-the-prototype/dsl-docs/blob/main/01_DSL_Goals_and_Principles.md)  
for details on motivation, goal setting, and design principles.  

Digital transformation in regulated environments has reached a critical point.  
Many public digitalization projects in Germany and Europe have demonstrated the limits of fragmented, incompatible, and non-auditable infrastructures — both technically and in terms of public trust and acceptance.

Following [**Auguste Kerkhoffs’ principle**](https://rock-the-prototype.com/en/cryptography/kerckhoff-principle/) — that security must rely on transparent, verifiable systems rather than obscurity — this project applies the same logic to **requirements engineering**. Software systems that govern public or regulated domains must be **traceable, testable, and independently auditable** from the first requirement onward.

### Why a DSL for Requirements?

The current practice of describing requirements in natural language or heterogeneous formats creates ambiguity and prevents automated validation.  

A domain-specific language (DSL) addresses this by providing:

- **Formal precision** – each requirement becomes a deterministic, testable atom.  
- **Auditability by design** – syntax and semantics enforce verifiability.  
- **Human and machine readability** – bridging expert review and automation.  
- **Governance integration** – embedding rule types, constraints, and exclusions.  

Our DSL defines the atomic unit of trust:  
a requirement that can be understood, verified, versioned, and audited — without depending on specific platforms or vendor interests.

### Context & Trust

True digital sovereignty does not come from building yet another isolated infrastructure but from **making existing ones transparent and auditable**.  
Instead of debating shortcomings, this initiative proposes a constructive, consensus-oriented approach that:

- enables voluntary participation and self-determined data control,  
- fosters interoperability through open, standard-based semantics, and  
- ensures that every rule, validation, and exception remains inspectable.  

In essence, **trust arises from transparency**, and transparency begins with the language we use to define what systems must — and must not — do.


# Getting Started
│
│
│                  
# Formal grammar, syntax principles, examples
│
├── 02_DSL_Grammar.md  
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
