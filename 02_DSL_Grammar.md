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

<modality> ::= "must" | "shall" | "must not" | "shall not"

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

