# Actuators

## 1. Overview

An actuator is a physical part of the pianist's body or piano that performs or participates in a musical action.

The Piano Programming Language describes performance from the pianist's physical perspective. Actuators therefore identify the physical agent used to perform an action rather than describing only the resulting sound.

The language defines four main actuator groups:

- Hands
- Fingers
- Feet
- Pedals

Actuator values are used as the `what` parameter of actions such as `press()`.

## 2. Hands

The two hands are represented by:

```text
LH
RH
```

- `LH` — left hand
- `RH` — right hand

A hand identifies the physical hand involved in an action.

## 3. Fingers

Individual fingers are represented by a hand letter followed by a finger number:

```text
L1 L2 L3 L4 L5
R1 R2 R3 R4 R5
```

The numbering is from left to right within each hand.

- `L1` through `L5` identify the five fingers of the left hand.
- `R1` through `R5` identify the five fingers of the right hand.

The language does not use forms such as `RH1` or `LH1`. The hand is encoded directly in the finger identifier.

## 4. Feet

The feet are represented by:

```text
LF
RF
```

- `LF` — left foot
- `RF` — right foot

Feet are actuators and may be used when describing physical pedal actions or other future actions involving the feet.

## 5. Pedals

The piano pedals are represented by:

```text
P1
P2
P3
```

Each pedal has its own actuator identifier.

The actuator identifier describes which pedal is involved. The detailed behavior of pedal actions belongs to the pedal specification.

## 6. Actuator Values

Actuator identifiers are values. They may be supplied directly to parameters that accept an actuator.

Example:

```c
press(R1, C4, ., normal, .);
```

Here `R1` identifies the right-hand finger performing the action.

A function or construct may also receive an actuator as a parameter and pass it to another construct.

## 7. Actuator Hierarchy

Actuator identifiers describe different levels of physical control:

```text
Hand
├── LH
│   ├── L1
│   ├── L2
│   ├── L3
│   ├── L4
│   └── L5
└── RH
    ├── R1
    ├── R2
    ├── R3
    ├── R4
    └── R5

Feet
├── LF
└── RF

Pedals
├── P1
├── P2
└── P3
```

The hierarchy is conceptual. It describes the relationship between actuator values and the physical parts they represent; it does not introduce additional actuator syntax.

## 8. Actuators and Physical Constraints

Actuators are subject to physical execution constraints.

For example, an actuator that is already performing an active action cannot necessarily begin another action at the same time. The execution model defines these constraints more precisely.

Different actuators may perform actions concurrently.

The actuator specification identifies the physical agents; timing and concurrency specifications define how their actions are scheduled.

## 9. Actuators in `press()`

The first parameter of `press()` identifies the actuator.

Example:

```c
press(R1, C4, ., normal, .);
```

The first argument is `R1`, meaning that the first finger of the right hand performs the press.

The general form is:

```c
press(what, where, duration, how, interval);
```

For `press()`, `what` is the actuator and `where` is the keyboard location.

## 10. Actuators in `together()`

`together()` can contain several actions that begin at the same time.

Example:

```c
together(., ., ., ., .) {
    press(R1, C4, ., normal, .);
    press(R2, D4, ., normal, .);
    press(R3, E4, ., normal, .);
};
```

Here three different finger actuators participate in the synchronized group.

The detailed synchronization rules belong to the `together()` specification.

## 11. Actuators in `repeat()`

`repeat()` may provide an actuator as part of its surrounding parameter context.

Example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

Within the repeated body, `.` can inherit the surrounding actuator value. Relative transformations may then produce related actuator values.

The detailed inheritance and transformation rules are defined in the corresponding specifications.

## 12. Inheritance and Actuators

The inheritance operator `.` can represent an actuator inherited from the surrounding context when the parameter position supports actuator inheritance.

For example:

```c
press(., ., ., ., .);
```

does not introduce a new actuator value. It requests the actuator value from the surrounding context.

This allows repeated physical patterns to be expressed without rewriting the same actuator values.

## 13. Relative Transformation of Actuators

Actuator values can participate in relative transformations.

For a finger identifier:

```text
R1
```

the finger-number component can be transformed:

```text
R1 → R2 → R3
```

using the appropriate relative form.

A transformation can also operate on another component of a compound actuator value, according to the relative-transformation rules.

The exact transformation syntax and semantics are defined in `02-002_RelativeTransformations.md`.

## 14. Actuator and Parameter Validity

An actuator value is valid only when it is used in a parameter that accepts that kind of actuator.

For example, `R1` is appropriate as the actuator argument of `press()`.

A keyboard value such as `C4` is not an actuator and therefore does not have the same parameter role.

The language uses parameter context to determine which values are valid.

## 15. Summary

The actuator model provides the physical vocabulary of the language:

```text
Hands:
    LH RH

Fingers:
    L1 L2 L3 L4 L5
    R1 R2 R3 R4 R5

Feet:
    LF RF

Pedals:
    P1 P2 P3
```

Actuators identify the physical agents of performance. They can be supplied directly, inherited through `.`, or transformed relatively where the language permits.

The actuator model is intentionally simple. More detailed behavior is defined by the specifications for keyboard values, pedals, expressions, timing, concurrency, and execution.
