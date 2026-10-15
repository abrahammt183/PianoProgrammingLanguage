# 06-000 Execution Model

## 1. Overview
The Execution Model defines the pipeline through which a high-level instruction in the language is transformed into a playable musical event. The process follows a strict sequence of parsing, parameter validation, and hardware/software synthesis mapping.

## 2. Execution Pipeline
The lifecycle of a single instruction follows these five stages:

1. Tokenization: Breaking the instruction string into discrete units.
2. Parameter Extraction: Identifying the five core parameters.
3. Validation: Ensuring parameters 3 (Duration) and 5 (Interval) are non-decimal and adhere to the grammar.
4. Context Mapping: Applying inherited parameters (via . notation) if the instruction is a derived function.
5. Synthesis Trigger: Sending the final command to the MIDI/Audio engine.

## 3. Core Parameter Mapping
Every atomic instruction must resolve to the following five-parameter structure:

| Parameter | Role | Description | Constraint |
| :--- | :--- | :--- | :--- |
| P1 | What | The finger/velocity/attack profile | Numeric/Symbolic |
| P2 | Where | The target musical note/pitch | Note Name/Index |
| P3 | Duration | How long the note is held | Integer Only |
| P4 | How | Non-numeric expression (e.g., legato, staccato) | String/Keyword |
| P5 | Interval | The rhythmic or melodic spacing | Integer Only |

## 4. Parameter Inheritance & Expansion
To maintain conciseness, the execution engine supports two types of command structures:

### 4.1 Atomic Commands
Directly provide all five parameters.
[P1] [P2] [P3] [P4] [P5]

### 4.2 Derived Commands (Dot Notation)
Extra functions inherit properties from a parent definition. The engine resolves the dots (.) by pulling the missing parameters from the parent's context.
* Rule: If a function is called as Parent.Child, the Child inherits all parameters of Parent unless explicitly overridden in its own definition.
* Expansion: If a repeat command is used, it acts as a sixth parameter, modifying the execution count of the preceding block.

## 5. Mathematical Representation of Execution
For any instruction I, the resulting sound state S can be represented as:

S = f(P1, P2, P3, P4, P5) * product(Ri)

Where:
- f is the synthesis function.
- P1-P5 are the core parameters.
- Ri represents the repetition factor (if applicable).

## 6. Error Handling during Execution
The execution engine must halt and throw a SyntaxError if:
- P3 is not an integer.
- P5 is not an integer.
- The number of parameters provided is less than 5 for an atomic command.
- A dot-notation reference points to a non-existent parent command.
