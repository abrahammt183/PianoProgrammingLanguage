# Repeat

## 1. Overview

`repeat()` executes a block of instructions multiple times.

It provides a way to describe repeated physical patterns without writing the same sequence of instructions separately for every occurrence.

The general form is:

```c
repeat(what, where, duration, how, interval, count) {
    ...
};
```

The first five parameters follow the general execution parameter order:

```text
what, where, duration, how, interval
```

The additional `count` parameter specifies how many times the body is repeated.

## 2. Parameters

The parameters of `repeat()` are:

```text
what
where
duration
how
interval
count
```

Their roles are:

- `what` — the initial actuator context.
- `where` — the initial keyboard-location context.
- `duration` — the duration context.
- `how` — the execution-mode context.
- `interval` — the interval context.
- `count` — the number of repetitions.

## 3. Basic Example

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

The block is executed four times.

The values supplied to `repeat()` establish the initial context for the repeated body.

## 4. Repetition Count

The `count` parameter determines how many times the body is executed.

For example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
};
```

has:

```text
count = 4
```

Therefore the body is executed four times.

A count is part of the repeat construct and is not one of the five general execution parameters.

## 5. Repeat Context

The first five parameters establish the context in which the repeated body executes.

For example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
};
```

The `press()` statement inherits the corresponding values from the repeat context.

The inheritance is positional.

## 6. Inheritance Inside `repeat()`

The symbol `.` can inherit a value from the current repeat context.

For example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
};
```

The `press()` instruction requests the corresponding values from the repeat context according to its parameter positions.

## 7. Relative Addressing

`repeat()` is the construct in which relative addressing is used to describe successive related values.

For example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

The relative expressions:

```text
.[+1]
[+1].
```

transform values relative to the current repeat context.

This allows a repeated pattern to move through related actuators or keyboard locations without writing every absolute value.

## 8. Actuator Transformation

A finger actuator can be transformed relative to the current actuator.

For example, a progression may be expressed as:

```text
R1 → R2 → R3
```

using the appropriate relative form.

The exact transformation semantics are defined in `02-002_RelativeTransformations.md`.

## 9. Keyboard Transformation

A keyboard value can also be transformed relative to the current keyboard location.

For example:

```text
C4
D4
E4
```

can be represented as a relative progression inside an appropriate repeated context.

The repeat example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

uses a relative keyboard transformation in the repeated body.

## 10. Relative Timing

Timing values may also be transformed relatively inside a repeat pattern.

Relative timing uses levels:

```text
[+1]
[+2]
[-1]
[-2]
```

For example:

```text
[+1]
```

doubles the current timing value, while:

```text
[-1]
```

halves it.

Relative timing is used inside repeated structures when a pattern requires timing to change in relation to the surrounding value.

## 11. Absolute and Relative Values

A repeat body may contain explicit values as well as inherited and relative values.

For example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

The first action uses inherited values.

The second action uses relative transformations for the actuator and keyboard location while inheriting the other parameters.

Absolute values may still be used when a repeated action needs a fixed value.

## 12. Execution Order

The body of a `repeat()` is executed in repetition order.

If:

```text
count = 4
```

the body executes as:

```text
iteration 1
iteration 2
iteration 3
iteration 4
```

Each iteration applies the repeat context and the expressions contained in the body according to the language's inheritance and transformation rules.

## 13. Asynchronous Actions

Instructions inside a repeat remain subject to the asynchronous execution model.

A repeated `press()` does not automatically wait for its duration to finish before the next instruction begins.

The interval determines when subsequent instructions begin.

Therefore, repeated actions may overlap when their duration and interval values produce overlapping active periods and the actuator constraints permit the overlap.

## 14. Actuator Constraints

Repeated execution does not remove physical actuator constraints.

For example, repeatedly assigning overlapping actions to the same actuator may be invalid if the actuator is still active when the next action begins.

The execution model and actuator-constraint specification determine whether a particular repeated pattern is physically valid.

## 15. Repeat and `together()`

`repeat()` and `together()` serve different purposes.

- `repeat()` executes a pattern multiple times.
- `together()` synchronizes the beginning of multiple actions.

A repeated body may contain multiple actions, and their synchronization behavior follows the rules of the constructs used inside the body.

## 16. Repeat and Functions

A function can contain a `repeat()` construct, allowing a repeated physical pattern to become reusable.

For example:

```c
function pattern(what, where, duration, how, interval) {
    repeat(what, where, duration, how, interval, 4) {
        ...
    };
};
```

Function parameters provide the surrounding values, while `repeat()` adds the repetition count.

## 17. Scope of Relative Addressing

Relative addressing is particularly useful inside `repeat()` because repeated iterations commonly describe movement from one value to another.

Outside a repeat context, absolute addressing should normally be preferred unless another specification explicitly permits relative addressing.

This keeps ordinary code explicit while allowing repeated patterns to express relationships compactly.

## 18. Syntax

The basic syntax is:

```c
repeat(what, where, duration, how, interval, count) {
    statement;
    statement;
    ...
};
```

The `count` parameter is placed after the five general execution parameters.

## 19. Summary

`repeat()` executes a block a specified number of times.

Its syntax is:

```c
repeat(what, where, duration, how, interval, count) {
    ...
};
```

Its important properties are:

- `count` determines the number of iterations;
- the first five parameters establish the repeat context;
- `.` inherits values from that context;
- relative addressing can describe successive related values inside the repeated pattern;
- relative timing can modify timing levels inside repeated patterns;
- actions remain asynchronous;
- actuator constraints still apply;
- `repeat()` provides repetition, while `together()` provides synchronization.
