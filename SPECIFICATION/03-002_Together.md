# Together

## 1. Overview

`together()` groups multiple actions that must begin at the same time.

It provides explicit synchronization for actions that would otherwise be separate instructions.

The general form is:

```c
together(what, where, duration, how, interval) {
    ...
};
```

The five parameters follow the general parameter order:

```text
what, where, duration, how, interval
```

## 2. Basic Example

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., normal, .);
    press(R3, E4, ., normal, .);
};
```

The three `press()` actions begin together.

## 3. Purpose

`press()` is asynchronous. Normally, separate instructions are scheduled according to their intervals.

`together()` explicitly states that several actions share the same starting point.

## 4. Group Parameters

`together()` uses:

```text
what
where
duration
how
interval
```

These establish the surrounding values for the actions inside the group.

The inheritance operator `.` can be used inside the group.

Example:

```c
together(RH, ., ., normal, .) {
    press(., C4, ., ., .);
    press(., E4, ., ., .);
    press(., G4, ., ., .);
};
```

## 5. Simultaneous Start

The defining property of `together()` is simultaneous start.

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, E4, ., normal, .);
};
```

Both presses begin at the start of the group.

Synchronization concerns their beginning, not necessarily their finishing time.

## 6. Different Durations

Actions inside a `together()` group may have different durations.

```c
together(., ., ., ., .) {
    press(R1, C4, 2., normal, .);
    press(R2, E4, ., normal, .);
};
```

Both actions start together, but `R1` remains active longer.

## 7. Interval

The `interval` parameter of `together()` establishes the group's scheduling relationship with the following instruction.

It does not require the individual actions to have identical durations.

## 8. Inheritance

`.` can inherit the corresponding value from the surrounding `together()` context.

```c
together(R1, C4, ., normal, .) {
    press(., ., ., ., .);
};
```

Inheritance is positional: each `.` refers to the corresponding parameter position.

## 9. Explicit Values

Values inside the group may also be supplied explicitly.

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., soft, .);
};
```

The individual actions specify their own values while still beginning together.

## 10. Actuator Constraints

Actions inside a `together()` group remain subject to actuator constraints.

For example, simultaneous independent actions normally use distinct actuators:

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, E4, ., normal, .);
};
```

The precise constraints are defined by the execution specifications.

## 11. No Nested `together()`

`together()` groups are not nested.

A `together()` block cannot contain another `together()` block.

## 12. Relationship to `press()`

`together()` does not replace `press()`.

It provides the synchronization context in which actions such as `press()` can begin together.

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., normal, .);
};
```

## 13. Relationship to `repeat()`

`together()` and `repeat()` have different purposes:

- `together()` synchronizes the start of multiple actions.
- `repeat()` executes a pattern multiple times.

The detailed repetition rules are defined in `03-003_Repeat.md`.

## 14. Relationship to Functions

A function may contain a `together()` statement, allowing a synchronized physical pattern to be reused.

For example:

```c
function chord(what, where, duration, how, interval) {
    together(what, where, duration, how, interval) {
        ...
    };
};
```

The exact function semantics are defined in the function specifications.

## 15. Syntax

The basic syntax is:

```c
together(what, where, duration, how, interval) {
    statement;
    statement;
    ...
};
```

The body contains actions that begin together.

## 16. Summary

`together()` is the explicit synchronization construct of the Piano Programming Language.

Its purpose is to make multiple actions begin simultaneously.

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., normal, .);
    press(R3, E4, ., normal, .);
};
```

Its important properties are:

- contained actions begin together;
- individual actions may have different durations;
- inheritance can provide surrounding parameter values;
- actions remain subject to actuator constraints;
- `together()` groups are not nested;
- `together()` synchronizes actions but does not replace the actions themselves.
