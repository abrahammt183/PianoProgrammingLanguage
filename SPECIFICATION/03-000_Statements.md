# Statements

## 1. Overview

A statement describes an executable instruction in the Piano Programming Language. Statements organize physical actions, synchronization, repetition, and calls to reusable functions.

The principal statement forms are:

- `press()` — start a physical press action.
- `together()` — start multiple actions simultaneously.
- `repeat()` — execute a pattern repeatedly.
- Function calls — invoke a previously defined reusable pattern.

The detailed semantics of each form belong to its dedicated specification. This document establishes their shared role and organization.

## 2. Statement Structure

A statement uses a construct name, positional arguments, and, where required, a body enclosed in braces. The common argument order is:

```text
what, where, duration, how, interval
```

A construct may append additional arguments. For example, `repeat()` adds `count`.

A simple action is written as a call:

```c
press(R1, C4, ., normal, .);
```

A compound statement includes a body:

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., normal, .);
    press(R3, E4, ., normal, .);
};
```

## 3. `press()` Statements

`press()` starts a physical action using the following argument order:

```c
press(what, where, duration, how, interval);
```

Example:

```c
press(R1, C4, ., normal, .);
```

The action begins when its statement executes. Its `duration` specifies how long the press remains active; its `interval` controls the timing of the next instruction. A `press()` statement does not necessarily wait until its action finishes before subsequent instructions begin.

See `03-001_Press.md` for the detailed rules.

## 4. `together()` Statements

`together()` groups actions that must begin at the same time:

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., normal, .);
    press(R3, E4, ., normal, .);
};
```

Its body contains the actions to synchronize. Their start times are shared, but their durations may differ. The group does not automatically wait for every action to finish.

The established design does not permit nested `together()` groups or duplicate actuators within the same group. See `03-002_Together.md` for the detailed rules.

## 5. `repeat()` Statements

`repeat()` executes a pattern multiple times. It uses the common five arguments followed by `count`:

```c
repeat(what, where, duration, how, interval, count) {
    // repeated statements
};
```

An example using inheritance and relative transformations:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

The `.` expressions inherit values from the surrounding context. Relative transformations express changes to those values within the repeated pattern. This example follows the established language overview; the precise evaluation of each repeated iteration belongs to `03-003_Repeat.md` and the expression specifications.

## 6. Function Calls

Functions provide reusable descriptions of physical patterns. A function call is an executable statement when it invokes such a pattern.

Function arguments follow the function's declared positional parameters. The established common order is `what`, `where`, `duration`, `how`, and `interval`, with additional parameters where needed.

Function definitions and calls are specified in `04-000_Functions.md`, `04-001_Parameters.md`, and `04-002_FunctionCalls.md`.

## 7. Inheritance and Statement Context

The symbol `.` inherits the value corresponding to its argument position from the surrounding context. A statement may therefore use explicit values, inherited values, or permitted relative transformations.

For example:

```c
press(R1, C4, ., normal, .);
```

The actuator, key, and mode are explicit; duration and interval are inherited.

Compound statements provide contexts in which their body statements can inherit values. See `02-001_Inheritance.md` for the rules governing nested contexts and shadowing.

## 8. Execution and Timing

Statement order and action completion are distinct concepts. In particular, `press()` begins an asynchronous action. The interval determines when the next instruction begins, which can be before the current action has finished.

A statement must still respect actuator constraints. The same actuator cannot start another incompatible press while its previous action remains active. Different actuators can act concurrently where permitted.

See `05-000_Timing.md`, `06-000_ExecutionModel.md`, `06-001_Concurrency.md`, and `06-002_ActuatorConstraints.md`.

## 9. Statement Boundaries and Bodies

The established examples use a semicolon after a standalone `press()` statement and after the closing brace of a compound `together()` or `repeat()` statement:

```c
press(R1, C4, ., normal, .);

together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., normal, .);
};
```

The complete grammar, including any additional valid statement forms and punctuation rules, belongs to `07-001_Grammar.md`.

## 10. Summary

Statements are the executable building blocks of a performance program. `press()` starts an action, `together()` synchronizes starts, `repeat()` reuses a pattern over iterations, and function calls invoke named reusable patterns. Positional parameters, inheritance, relative transformations, and asynchronous execution allow these simple forms to express more complex physical performances.
