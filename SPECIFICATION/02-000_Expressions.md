# Expressions

## 1. Overview

An **expression** supplies or derives a value used by a Piano Programming Language construct. Expressions describe values; statements such as `press()`, `together()`, and `repeat()` describe actions and control their execution.

Expressions support three ways to specify a value:

- **Explicit values**, such as `R1` or `C4`.
- **Inherited values**, represented by `.` in a parameter position.
- **Relative transformations**, which derive a value from another value or the surrounding context.

This document introduces these forms. Their detailed rules belong to `02-001_Inheritance.md` and `02-002_RelativeTransformations.md`.

## 2. Expressions and Parameter Positions

The general parameter order is:

```text
(what, where, duration, how, interval)
```

An expression is interpreted in its parameter position. For example:

```c
press(R1, C4, ., normal, .);
```

Here `R1` supplies the actuator (`what`), `C4` supplies the keyboard location (`where`), and `normal` supplies `how`. The two `.` expressions request the duration and interval from the surrounding context.

A construct may append additional parameters. For example, `repeat()` also has a `count` parameter.

## 3. Explicit Values

An explicit expression names a value directly. Established value categories include:

| Category       | Examples                                 | Purpose                                      |
| -------------- | ---------------------------------------- | -------------------------------------------- |
| Actuator       | `LH`, `RH`, `L1`, `R1`, `LF`, `RF`, `P1` | Identifies a physical agent or pedal.        |
| Keyboard       | `C4`, `D4`, `E4`                         | Identifies a key.                            |
| Timing         | `2.`, `.2`, `.4`                         | Supplies a duration or interval.             |
| Execution mode | `normal`, `soft`                         | Supplies a mode where supported.             |
| Count          | `4`                                      | Supplies a repetition count where supported. |

The complete rules for each value category are defined in its corresponding specification. An expression's validity depends on the parameter receiving it.

## 4. Inherited Expressions

The expression `.` means **inherit the corresponding value from the surrounding context**. It is positional: a dot in `what` inherits `what`; a dot in `where` inherits `where`, and so on.

```c
press(., ., ., ., .);
```

This action requests all five values from its surrounding context. The dot does not identify a single universal value; its meaning depends on its position and the available context.

**Context matters:** in an action's parameter list, `.` is an inheritance expression. Timing notation also uses `.` as a duration value in its own defined context. The detailed interpretation and any ambiguity rules belong to the lexical, inheritance, and timing specifications.

## 5. Relative Expressions

A relative expression derives a value by transforming one of its components. Established examples include:

```text
C[+1]4    → D4
C4[+1]    → C5
C[+1.5]4  → a black-key position
```

The placement of the transformation identifies which component changes. Relative forms can also act on inherited values. For example, within a repeated pattern:

```text
.[+1]
[+1].
```

The meaning depends on the inherited value's structure and the parameter position. For a finger value such as `R1`, a transformation of its numeric component can produce `R2`. For a keyboard value such as `C4`, a transformation of its key component can produce `D4`.

Relative addressing is used in repeated patterns in the established examples; ordinary action examples use explicit values or simple inheritance.

## 6. Expressions in a Repeated Pattern

The following example preserves the pattern established in the language overview:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

The body uses inheritance to avoid repeating the surrounding values and relative expressions to describe changes between related actions. The `repeat()` specification defines how iterations execute; the inheritance and transformation specifications define how the expressions are resolved.

## 7. Expressions and Functions

A function can receive values through parameters and use them in its body. Parameter names refer to the values supplied to the function in their respective positions.

```c
function pattern(what, where, duration, how, interval) {
    press(what, where, duration, how, interval);
};
```

Functions make it possible to reuse a physical pattern with different actuators, keyboard locations, and timing values. The rules for parameter binding, local scope, and calls belong to the function specifications.

## 8. Evaluation and Validity

Before an action can use an expression, the expression must resolve to a value accepted by that parameter. For example, a keyboard location belongs in `where`, while a finger actuator belongs in `what`.

An inherited expression requires an appropriate surrounding value. A relative expression requires a base value and a transformation applicable to the selected component. The error-handling specification defines how invalid or unresolvable expressions are reported.

This introductory document does **not** establish a general arithmetic language, operator precedence, implicit conversions, or unrestricted expression nesting. Such features must be defined explicitly before implementations rely on them.

## 9. Related Specifications

- `00-004_ValuesAndParameters.md` — value categories and positional parameters.
- `02-001_Inheritance.md` — resolution of `.` and surrounding contexts.
- `02-002_RelativeTransformations.md` — component-based relative expressions.
- `03-000_Statements.md` — constructs that consume expressions.
- `04-001_Parameters.md` — function parameter binding.
- `05-000_Timing.md` — timing values and transformations.
- `08-000_ErrorHandling.md` — invalid expressions and unresolved values.

## 10. Summary

Expressions supply values to the language's physical-action constructs. They can be explicit, inherited, or relative. Their interpretation is determined by parameter position, surrounding context, and the rules for the relevant value category. The language keeps expressions focused on describing physical performance rather than introducing unrelated general-purpose computation.
