# 07-000 Syntax Reference

## 1. Overview
The `Syntax Reference` serves as a consolidated guide for the formal structure of the Piano Programming Language. It provides the high-level grammar rules, token definitions, and structural patterns required to construct valid programs. This chapter is intended as a quick-lookup resource for developers.

## 2. Core Syntax Components
All valid programs are composed of three fundamental elements: **Tokens**, **Statements**, and **Functions**.

### 2.1. Tokens
Tokens are the smallest units of the language:
- **Identifiers:** Actuators (e.g., `R1`, `L1`), Notes (e.g., `C4`, `R1`), and Function names.
- **Literals:** Temporal values (e.g., `10ms`, `1s`) and qualitative values (e.g., `staccato`, `legato`).
- **Delimiters:** Commas (`,`), Parentheses (`(`, `)`), and Dots (`.`).
- **Operators:** The transformation operator `[]`.

### 2.2. Statement Structure
A standard statement follows the command-parameter pattern. The core commands (e.g., `press`, `together`, `repeat`) act as the verbs of the language.

**General Statement Template:**
`[COMMAND] [ARG1], [ARG2], [ARG3], [ARG4], [ARG5];`

*Note: The semicolon `;` marks the end of a statement.*

### 2.3. Function Invocation
Functions are invoked by their name followed by an argument list.

**Invocation Template:**
`[FUNCTION_NAME]([ARG1], [ARG2], [ARG3], [ARG4], [ARG5]);`

## 3. Grammar Hierarchy
The language follows a hierarchical structure for parsing:

1.  **Program Level:** A sequence of one or more statements.
2.  **Statement Level:** A single command or function call ending in a semicolon.
3.  **Argument Level:** A list of tokens separated by commas, where each token matches the expected type (Actuator, Note, Duration, etc.) defined in `00-004_ValuesAndParameters.md`.

## 4. The Dot (`.`) Notation in Grammar
The dot operator is used exclusively in function signatures to denote parameter inheritance and transformation.

- **Signature Syntax:** `func(., ., [TRANSFORM], ., .)`
- **Transformation Syntax:** `[operator][value]` (e.g., `[+1]`, `[-2]`).

## 5. Chapter Roadmap
This chapter is divided into the following detailed technical specifications:
- `07-001_Grammar.md`: Formal EBNF (Extended Backus-Naur Form) definition of the language.
- `07-002_Examples.md`: Practical code samples demonstrating various syntax patterns.

## 6. References
- `00-003_LexicalStructure.md` — Detailed tokenization rules.
- `04-001_Parameters.md` — Data types and positional rules.
- `04-002_FunctionCalls.md` — Formal rules for function invocation.
