# dsl-docs
Documentation and rationale for the Audit-by-Design DSL Explains the principles, design decisions, and use cases of the domain-specific language for defining, validating, and auditing atomic requirements (AFOs) in regulated software systems.

We are developing a **domain-specific language (DSL)** that enables the **atomization, validation, and traceability of requirements (AFOs)** in regulated software systems and above.

This language is designed to:

- be **human- and machine-readable** (structured text or JSON),
- represent **syntactically unambiguous and verifiable requirements**,
- be **testable, versionable, and auditable** (via Git integration),
- support **automatic validation and auditing** (CI/CD),
- and **semantically represent governance rules, negative rules, and exclusions**.


Design Principles

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



dsl-docs/
├── README.md                             
# Overview & Getting Started
│ 
├── 01_DSL_Goals_and_Principles.md        
# Motivation, goal setting, design principles
│
├── 02_DSL_Grammar.md                     
# Formal grammar, syntax principles, examples
│
├── 03_Validation_and_Audit.md            
# Parser/Validator, audit concept
│
├── 04_Examples.md    
│
# DSL-Examples (Text + JSON)
├── 05_Governance_and_Rules.md    
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
