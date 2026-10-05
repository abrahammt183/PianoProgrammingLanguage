# Design Principles

## 1. Physical Perspective

The language represents piano performance from the pianist's perspective.

Its primary concern is not merely which sounds occur, but which physical actions produce those sounds.

## 2. Simplicity

A small number of general mechanisms should express many musical situations.

The language should avoid separate syntax for every musical technique when inheritance, functions, synchronization, and transformation can express the same idea naturally.

## 3. Inheritance

Repeated values should not need to be written repeatedly.

The symbol `.` inherits the corresponding value from the surrounding context.

Example:

```c
press(R1, C4, ., soft, .);
```

The duration and interval are inherited from the current context.

## 4. Relative Transformation

Values may be transformed relative to their current values.

Examples:

```c
.[+1]
.[+2]
[-1].
```

These transformations may operate on different components of a value.

For example:

```c
R1 → R2 → R3
C4 → D4 → E4
```

The notation allows a sequence of related actions to be expressed without repeating their absolute values.

## 5. Asynchronous Actions

A `press()` action begins immediately and remains active for its specified duration.

The interval determines when the next instruction begins. The next instruction does not necessarily wait for the previous action to finish.

This allows overlapping notes, sustained tones, and independent physical actions.

## 6. Explicit Synchronization

Actions that begin together should be grouped explicitly.

The `together()` construct synchronizes the beginning of multiple actions while allowing each action to have its own duration.

## 7. Reusability

Common musical and physical patterns should be representable as functions.

A function may accept parameters and use inherited or transformed values to describe variations of the same gesture.

## 8. Human Readability

Source code should remain understandable to a pianist reading it directly.

The notation should favor meaningful structure and consistency over compactness for its own sake.

## 9. Separation of Specification and Implementation

The language definition must not depend on a particular interpreter, editor, playback engine, or storage format.

The specification defines the language first. Implementations may follow later.

## 10. Bottom-Up Learning

The language supports learning physical actions from simple components toward complex musical structures.

A beginner may study individual fingers and keys, while an advanced pianist may work with reusable functions, transformations, and complete passages.

> Learn from the bottom up; play from the top down.

