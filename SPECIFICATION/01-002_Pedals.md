# Pedals

## 1. Overview

Pedals are physical actuators represented by the Piano Programming Language.

The language identifies three piano pedals:

```text
P1
P2
P3
```

Each pedal has its own actuator identifier.

This specification defines the pedal identifiers. Detailed pedal actions and their execution behavior are not defined here unless specified elsewhere in the language specification.

## 2. Pedal Identifiers

The three pedals are represented as:

```text
P1
P2
P3
```

These identifiers distinguish the three pedals from one another.

They are actuator values, just as `LF` and `RF` identify the left and right feet.

## 3. Pedals and Feet

The language distinguishes between the physical foot and the pedal being operated.

The feet are:

```text
LF
RF
```

The pedals are:

```text
P1
P2
P3
```

Thus:

```text
LF
RF
```

identify the physical actuators of the pianist, while:

```text
P1
P2
P3
```

identify the piano pedals.

This distinction allows the language to describe the physical performer separately from the piano component being operated.

## 4. Pedals as Actuator Values

Pedal identifiers belong to the actuator value category.

They may therefore be used wherever a construct accepts an actuator value, subject to the rules of that construct.

For example:

```text
P1
```

identifies the first pedal.

The exact operation performed on a pedal is determined by the action that uses the pedal and by the corresponding execution rules.

## 5. Pedal Identity

A pedal identifier identifies the pedal itself.

The identifiers are distinct:

```text
P1
P2
P3
```

The language does not assign additional semantic meaning to these identifiers in this specification.

In particular, this specification does not define a particular physical pedal function for `P1`, `P2`, or `P3`.

## 6. Inheritance

The inheritance symbol `.` may represent a pedal value when used in a parameter position whose surrounding context provides a pedal value.

For example, a construct may receive a pedal as part of its context and use:

```text
.
```

to inherit that value.

The general rules for inheritance are defined by the language's inheritance specification.

## 7. Relative Transformation

Pedal identifiers are actuator values, but this specification does not define any pedal-specific relative transformation.

Any transformation applied to an actuator must follow the general relative-transformation rules.

## 8. Scope

This specification establishes the pedal vocabulary:

```text
P1
P2
P3
```

It does not define:

- the physical function of each pedal;
- a pedal press or release operation;
- pedal duration;
- pedal timing;
- pedal-specific execution constraints.

Those matters require corresponding language rules or constructs and should be defined in their appropriate specifications.

## 9. Summary

The Piano Programming Language represents the three piano pedals with:

```text
P1
P2
P3
```

They are actuator values and are distinct from the foot actuators:

```text
LF
RF
```

The pedal specification intentionally defines only the pedal identifiers and their role in the actuator model. Additional pedal behavior belongs to the specifications that define the relevant actions and execution semantics.
