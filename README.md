# dsl-docs
Documentation and rationale for the Audit-by-Design DSL Explains the principles, design decisions, and use cases of the domain-specific language for defining, validating, and auditing atomic requirements (AFOs) in regulated software systems.

We are developing a **domain-specific language (DSL)** that enables the **atomization, validation, and traceability of requirements (AFOs)** in regulated software systems and above.

This language is designed to:

- be **human- and machine-readable** (structured text or JSON),
- represent **syntactically unambiguous and verifiable requirements**,
- be **testable, versionable, and auditable** (via Git integration),
- support **automatic validation and auditing** (CI/CD),
- and **semantically represent governance rules, negative rules, and exclusions**.

dsl-docs/
├── README.md                             # Overview & Getting Started
├── 01_DSL_Goals_and_Principles.md        # Motivation, goal setting, design principles
├── 02_DSL_Grammar.md                     # Formal grammar, syntax principles, examples
├── 03_Validation_and_Audit.md            # Parser/Validator, audit concept
├── 04_Examples.md                        # DSL-Examples (Text + JSON)
├── 05_Governance_and_Rules.md            # Change processes, compliance, versioning
└── assets/
    ├── diagrams/
    │   └── dsl-cfg-structure.png         # later CFG graphic
    └── examples/
        └── requirement_atom.json         # JSON example for reference


## License

The **Audit-by-Design DSL Docs ** are released under  
[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).  
© 2025 Sascha Block / Rock the Prototype
