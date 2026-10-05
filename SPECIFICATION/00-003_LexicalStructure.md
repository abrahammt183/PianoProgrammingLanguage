# Lexical Structure

## 1. Purpose

This document defines the lexical elements of Piano Programming Language.

Lexical elements are the smallest meaningful units from which source code is constructed.

The lexical structure defines:

* identifiers;
* numeric values;
* musical values;
* actuator names;
* key names;
* punctuation;
* comments;
* whitespace;
* inheritance notation;
* and transformation notation.

This document does not define the complete meaning of statements or functions. Those semantics are defined in later specification documents.

---

# 2. Source Code

A Piano Programming Language source file consists of a sequence of characters encoded as Unicode text.

Source code is interpreted as a sequence of tokens.

Tokens may be separated by whitespace or comments where the grammar permits separation.

Whitespace is generally insignificant except where it separates otherwise ambiguous tokens.

Example:

```c
press(R1, C4, ., soft, .);
```

The following formatting is equivalent:

```c
press (
    R1,
    C4,
    .,
    soft,
    .
);
```

---

# 3. Identifiers

An identifier names a function, parameter, variable, or other language-defined entity.

An identifier consists of:

* a letter or underscore as its first character;
* followed by zero or more letters, digits, or underscores.

Examples:

```text
press
together
repeat
note
duration
interval
```

Identifiers are case-sensitive.

The following identifiers are distinct:

```text
note
Note
NOTE
```

Reserved keywords are defined by the language grammar.

---

# 4. Keywords

The following words are reserved for built-in language constructs:

```text
const
function
press
together
repeat
```

Additional keywords may be introduced by future versions of the specification.

A reserved keyword must not be used as a user-defined identifier unless explicitly permitted by a future language rule.

---

# 5. Numeric Literals

Numeric literals represent numeric values.

Examples:

```text
0
1
2
4
1.5
0.5
```

Numeric literals may be used for parameters that accept numeric values.

The exact interpretation of a numeric parameter depends on its context.

For example:

```c
count = 4;
```

represents a count, while:

```c
duration = 0.5;
```

represents a numeric duration value.

---

# 6. Duration Literals

Piano Programming Language provides musical-duration notation.

The global timing constant is declared as:

```c
const time = 1'';
```

The value of `time` defines one crotchet.

Duration literals are expressed relative to this constant.

| Literal | Musical value |
| ------- | ------------: |
| `.`     |    1 crotchet |
| `2.`    |   2 crotchets |
| `4.`    |   4 crotchets |
| `.2`    |    ½ crotchet |
| `.4`    |    ¼ crotchet |
| `.8`    |    ⅛ crotchet |
| `.16`   | 1/16 crotchet |
| `.32`   | 1/32 crotchet |

Examples:

```c
press(R1, C4, ., soft, .);
press(R2, D4, 2., ., .);
press(R3, E4, .2, ., .);
```

Duration literals are not ordinary decimal numbers.

The period has a structural meaning:

* a leading period represents a fractional subdivision;
* a trailing period represents multiplication of the base unit.

---

# 7. Inheritance Token

The period character `.` is also used as an inheritance token.

An inheritance token means:

> Use the corresponding value from the current surrounding context.

Example:

```c
together(R1, C4, ., soft, .) {
    press(., ., 4., ., .);
}
```

The inherited values are:

```text
what     = R1
where    = C4
duration = 4.
how      = soft
interval = .
```

Inheritance applies positionally.

For example:

```c
press(R1, C4, 4., soft, .);
press(., D4, ., ., .);
```

The second action inherits:

```text
what     = R1
duration = 4.
how      = soft
interval = .
```

while explicitly replacing:

```text
where    = D4
```

The inheritance token may also inherit an individual component of a compound value.

---

# 8. Relative Transformation Tokens

Relative transformations modify an existing value.

The general syntax is:

```text
[+n]
[-n]
```

where `n` is a numeric offset.

A transformation may be applied to a complete value or to one component of a compound value.

Examples:

```text
.[+1]
.[+2]
.[-1]
```

The transformation is resolved relative to the current inherited value.

For example:

```c
together(R1, C4, ., soft, .) {
    press(., ., 4., ., .);
    press(.[+1], [+1]., 3., ., .);
}
```

The second action transforms:

```text
R1 → R2
C4 → D4
```

---

# 9. Compound Values

Some values consist of multiple components.

Examples include:

```text
R1
C4
```

These are composed of:

```text
R1 = R + 1
C4 = C + 4
```

A relative transformation may apply to an individual component.

## 9.1 Actuator Components

For finger identifiers:

```text
R1
R2
R3
R4
R5
```

the actuator component is the hand:

```text
R
```

and the numeric component is the finger:

```text
1
2
3
4
5
```

Therefore:

```text
R1.[+1]
```

is interpreted as:

```text
R2
```

The transformation applies to the numeric component while preserving the hand.

## 9.2 Key Components

For key identifiers:

```text
C4
D4
E4
```

the key consists of:

```text
C + 4
D + 4
E + 4
```

A transformation may modify the pitch component or octave component independently.

Examples:

```text
.[+1].
.[+2].
```

may transform:

```text
C4 → D4
C4 → E4
```

while:

```text
.[+1]
```

applied to the octave component transforms:

```text
C4 → C5
```

The exact component-selection syntax is defined in the values and transformation specifications.

---

# 10. Actuator Identifiers

The language defines identifiers for physical actuators.

## 10.1 Hands

```text
LH
RH
```

represent the left and right hands.

## 10.2 Fingers

Left-hand fingers:

```text
L1
L2
L3
L4
L5
```

Right-hand fingers:

```text
R1
R2
R3
R4
R5
```

Spatial ordering is:

```text
L5 L4 L3 L2 L1 | R1 R2 R3 R4 R5
```

The individual finger numbers are defined from left to right within each hand.

## 10.3 Feet

```text
LF
RF
```

represent the left and right feet.

## 10.4 Pedals

```text
P1
P2
P3
```

represent the available pedals.

The physical meaning of each pedal may depend on the instrument configuration.

---

# 11. Key Identifiers

Piano keys are identified using a letter and octave number.

White keys use uppercase letters:

```text
A0
B0
C1
D1
E1
F1
G1
...
C8
```

The supported piano range is:

```text
A0–C8
```

## 11.1 Black Keys

Black keys use a lowercase letter corresponding to the next white key.

Examples:

| Black key | Meaning   |
| --------- | --------- |
| `d4`      | C♯4 / D♭4 |
| `e4`      | D♯4 / E♭4 |
| `g4`      | F♯4 / G♭4 |
| `a4`      | G♯4 / A♭4 |
| `b4`      | A♯4 / B♭4 |

Black-key notation is intentionally based on the physical keyboard position rather than enharmonic spelling.

---

# 12. Text Literals

Text literals may be used for descriptive or symbolic parameters where permitted by the grammar.

Example:

```c
soft
legato
staccato
```

The language may later define a formal set of recognized performance descriptors.

Unrecognized descriptors may be rejected by an implementation or treated according to future extensibility rules.

---

# 13. Punctuation

The following punctuation characters have syntactic meaning:

| Symbol  | Meaning                          |
| ------- | -------------------------------- |
| `(` `)` | Parameter list or grouping       |
| `{` `}` | Block                            |
| `,`     | Parameter separator              |
| `;`     | Statement terminator             |
| `.`     | Inheritance or duration notation |
| `[` `]` | Relative transformation          |
| `+`     | Positive transformation          |
| `-`     | Negative transformation          |
| `'`     | Time-unit notation               |

Example:

```c
press(R1, C4, ., soft, .);
```

---

# 14. Comments

Comments are non-executable source text.

A single-line comment begins with:

```text
//
```

and continues until the end of the line.

Example:

```c
press(R1, C4, ., soft, .); // Begin with the thumb.
```

A multi-line comment begins with:

```text
/*
```

and ends with:

```text
*/
```

Example:

```c
/*
    This passage contains
    a descending finger pattern.
*/
```

Comments have no effect on execution.

---

# 15. Whitespace

Whitespace includes spaces, tabs, and line breaks.

Whitespace may be inserted between tokens.

Example:

```c
press(R1,C4,.,soft,.);
```

and:

```c
press(
    R1,
    C4,
    .,
    soft,
    .
);
```

represent the same lexical structure.

Whitespace must not split a token into invalid fragments.

---

# 16. Statement Terminators

A semicolon terminates an executable statement.

Example:

```c
press(R1, C4, ., soft, .);
```

Statements inside a block are normally separated by semicolons.

Example:

```c
together(R1, C4, ., soft, .) {
    press(., ., 4., ., .);
    press(.[+1], [+1]., 3., ., .);
}
```

The rules governing whether a semicolon is required after blocks are defined by the statement grammar.

---

# 17. Lexical Ambiguity

The period character has multiple meanings:

1. duration notation;
2. inheritance;
3. part of a relative transformation expression.

Its meaning is determined by syntactic context.

Examples:

```text
.
```

may mean inheritance or one crotchet, depending on the parameter position.

```text
2.
```

is a duration literal.

```text
.[+1]
```

is a transformed inherited value.

Implementations must resolve these meanings according to the grammar and surrounding parameter context.

---

# 18. Summary

The lexical structure establishes a compact notation based on a small number of symbols.

The most important lexical mechanisms are:

```c
press(what, where, duration, how, interval);
```

```text
.
```

for inheritance,

```text
[+n]
[-n]
```

for relative transformations,

and:

```text
together(...)
repeat(...)
function ...
```

for higher-level performance structures.

These lexical elements provide the foundation for the semantic rules defined in the following specification documents.

