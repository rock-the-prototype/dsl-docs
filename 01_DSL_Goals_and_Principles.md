# 1. Goal of the DSL

We are developing a **domain-specific language (DSL)** that enables the **atomization, validation, and traceability of requirements (AFOs)** in regulated software systems and above.

This language is designed to:

- be **human- and machine-readable** (structured text or JSON),
- represent **syntactically unambiguous and verifiable requirements**,
- be **testable, versionable, and auditable** (via Git integration),
- support **automatic validation and auditing** (CI/CD),
- and **semantically represent governance rules, negative rules, and exclusions**.


---

# 2. Design Principles

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

---
