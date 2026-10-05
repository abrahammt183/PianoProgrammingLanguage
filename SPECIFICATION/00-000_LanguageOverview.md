# Piano Programming Language

## Language Overview

### Purpose

The Piano Programming Language is a pianist-oriented notation and programming language for describing how a musical performance is physically executed.

It is designed to represent instructions close to the actions sent to the hands and feet rather than to reproduce traditional staff notation. The notation treats performance as a sequence of commands, parameters, reusable functions, and nested structures.

The central idea is:

> Musical performance can be described as executable instructions for physical actuators.

The language is intended to make piano practice and performance easier to understand, remember, write, and execute.

## Design Goals

The language is designed around the following goals:

1. **Pianist perspective**
   The notation should reflect the player's physical actions: which actuator acts, which key or pedal is involved, how long it acts, and how actions relate to one another.

2. **Executable structure**
   Musical instructions should be expressible using programming concepts such as functions, parameters, nesting, inheritance, and repetition.

3. **Brain-friendly notation**
   The notation should reduce unnecessary visual and conceptual translation between written instructions and physical movement.

4. **Abstraction**
   Repeated or complex movements should be representable as reusable functions without losing the ability to inspect their concrete actions.

5. **Precise timing**
   Durations and intervals should use a consistent time model that scales naturally when the tempo changes.

6. **Simple syntax**
   The language should remain small, readable, and close to familiar programming notation.

## Core Model

The language describes four main kinds of information:

- **Actuator** — the body part or mechanism performing an action.
- **What** — the action to perform, such as `press`.
- **Where** — the target, such as a piano key or pedal.
- **When and how** — duration, interval, and other execution parameters.

The primary keyboard actuators are the left and right hands:

- `LH` — left hand
- `RH` — right hand

Each hand has five fingers, numbered from left to right:

- `L1` through `L5`
- `R1` through `R5`

The feet and pedals are also available:

- `LF` — left foot
- `RF` — right foot
- `P1`, `P2`, `P3` — pedals

## Basic Instruction

The fundamental action is `press`.

A conceptual form is:

```text
press(what, where, duration, how, interval)
```

The exact arguments may be inherited or omitted when the surrounding structure supplies them.

For example:

```text
press(R1, C4, ., normal, .);
```

This describes pressing key `C4` with right-hand finger `R1` for one crotchet in normal mode and start the next note in a crotchet.

The language treats a press as asynchronous: after an actuator presses a key, the actuator continues holding it while later instructions are read and executed. A later instruction may therefore involve another actuator while the first actuator remains active.

## Simultaneous Actions

The `together` construct groups actions that begin together.

```text
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., normal, .);
    press(R3, E4, ., normal, .);
};
```

The grouped presses form one simultaneous chord-like action.

A `together` block may share inherited parameters from its parent. The block is intended for simultaneous execution, not merely for visually grouping sequential instructions.

## Repetition

The `repeat` construct expresses repeated execution without copying the same instruction multiple times.

Conceptually:

```text
repeat(what, where, duration, how, interval, count) {
    press(...)
}
```

For example:

```text
repeat(R1, C4, ., R1, .2, 4) {
    press (., ., ., ., .);
    press (.[+1], [+1]., ., ., .)
    };
```

The precise syntax and inheritance rules are defined in the later specification documents.

## Functions

Functions provide reusable movement patterns.

A function may have parameters and may contain nested statements:

```text
function arpeggio (what, where, duration, how, interval, count){
    press (what, where, duration, how, interval, count);
};
```

Functions are used to separate a general movement pattern from the concrete values used at a particular location in a piece.

Function parameters are positional and local to the function. Calling a function supplies concrete values for those parameters.

## Values and Inheritance

The language supports explicit values, inherited values, and relative transformations.

A child instruction may omit an argument when it should inherit the corresponding value from its parent.

The dot notation represents inheritance:

```text
press(C4, R1, ., .)
```

A relative transformation changes an inherited or referenced value rather than replacing it with an unrelated absolute value.

Examples of the intended style include:

```text
C[+1]4
C4[+1]
C[+1.5]4
```

These represent relative movement in pitch notation according to the language's keyboard-oriented rules.

The complete transformation rules are specified separately in the values and inheritance documents.

## Keyboard Notation

White keys use conventional uppercase note letters with octave numbers:

```text
C4
D4
E4
F4
G4
A4
B4
```

Black keys use lowercase names based on the next white key:

```text
b0
d1
f1
a1
```

The final naming rules and all accepted keyboard coordinates are defined in the keyboard specification.

## Timing

Timing is based on one global constant:

```text
const time = 1''
```

This defines one second as one crotchet.

Duration and interval values use time tokens:

| Token | Meaning                 | Relative duration |
|-------|-------------------------|-------------------|
| `.`   | crotchet                | 1 second          |
| `2.`  | minim                   | 2 seconds         |
| `4.`  | semibreve               | 4 seconds         |
| `.2`  | quaver                  | 1/2 second        |
| `.4`  | semiquaver              | 1/4 second        |
| `.8`  | demisemiquaver          | 1/8 second        |
| `.16` | hemidemisemiquaver      | 1/16 second       |
| `.32` | quasihemidemisemiquaver | 1/32 second       |

The same time-token system is used for durations and waits or intervals. Tempo can therefore be changed by changing the value of the global time constant rather than rewriting every instruction.

## Execution Rules

The execution model distinguishes between:

- actions that are started,
- actions that remain active,
- actions that finish after their duration,
- and actions that begin at a specified interval.

A single actuator cannot press a new key while its previous key is still active unless the language explicitly provides a mechanism for that behavior. This prevents contradictory finger instructions.

Different actuators may act concurrently.

The execution rules also define how nested blocks, function calls, repetition, inheritance, and simultaneous actions interact.

## Scope of the Language

The Piano Programming Language is intended to describe physical performance instructions. It is not primarily a replacement for staff notation, music theory notation, or an audio file format.

A complete program may eventually contain:

- declarations,
- actuator definitions,
- keyboard and pedal targets,
- functions,
- parameterized movement patterns,
- simultaneous actions,
- repetitions,
- timing,
- and execution constraints.

The language specification is organized into separate documents so that each part can be developed independently while preserving a coherent overall design.

## Specification Organization

The specification uses numbered Markdown files.

The overview is:

- `00-000_LanguageOverview.md` — purpose, philosophy, and complete high-level model.
- `00-001_DesignPrinciples.md` — design principles and constraints.
- `00-002_LexicalStructure.md` — tokens, identifiers, literals, and comments.
- `00-003_ValuesAndParameters.md` — values, parameters, inheritance, and references.
- `01-000_Actuators.md` — actuator model.
- `01-001_Keyboard.md` — keyboard keys and finger notation.
- `01-002_Pedals.md` — feet and pedal notation.
- `02-000_Expressions.md` — expressions and value composition.
- `02-001_Inheritance.md` — inherited arguments and defaults.
- `02-002_RelativeTransformations.md` — relative value transformations.
- `03-000_Statements.md` — statement model.
- `03-001_Press.md` — press statement.
- `03-002_Together.md` — simultaneous execution.
- `03-003_Repeat.md` — repetition.
- `04-000_Functions.md` — function model.
- `04-001_Parameters.md` — function parameters.
- `04-002_FunctionCalls.md` — function invocation.
- `05-000_Timing.md` — global time model.
- `05-001_Duration.md` — duration values.
- `05-002_Interval.md` — intervals and waits.
- `06-000_ExecutionModel.md` — execution semantics.
- `06-001_Concurrency.md` — concurrent actions.
- `06-002_ActuatorConstraints.md` — restrictions on active actuators.
- `07-000_SyntaxReference.md` — syntax summary.
- `07-001_Grammar.md` — formal grammar.
- `07-002_Examples.md` — complete examples.
- `08-000_ErrorHandling.md` — invalid programs and execution errors.
- `09-000_StandardLibrary.md` — reusable standard definitions.

This overview is intentionally descriptive. Detailed syntax, formal grammar, and normative execution behavior belong in the specialized specification documents.
