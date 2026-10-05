# Values and Parameters

## 1. Overview

The Piano Programming Language uses values and parameters to describe physical actions and reusable movement patterns.

A parameter gives a construct a place where a value can be supplied. A value supplies the concrete information for that parameter.

Parameters are positional. Their meaning is determined by the parameter's position within the construct.

The general parameter order is:

```text
what, where, duration, how, interval
```

Additional parameters may be appended when required by a construct, such as `count` for repetition.

## 2. Parameters

A parameter is a named position in a function or construct.

For example:

```c
function arpeggio(what, where, duration, how, interval, count) {
    ...
};
```

The function has six parameters:

```text
what
where
duration
how
interval
count
```

A parameter does not itself specify a concrete key, finger, duration, or other value. It defines where that value will be supplied.

## 3. Positional Meaning

Parameters are interpreted by position.

For example:

```c
press(R1, C4, ., normal, .);
```

corresponds to:

```text
what     = R1
where    = C4
duration = .
how      = normal
interval = .
```

The exact parameter meanings belong to the construct being called. The parameter order must remain consistent with that construct's definition.

## 4. Values

Values provide concrete information for parameters.

The language includes several categories of values, including:

- actuator values;
- keyboard values;
- timing values;
- execution-mode values;
- numeric values;
- inherited values;
- and transformed values.

The same lexical form may have different semantic roles depending on the parameter receiving it.

For example:

```text
R1
```

is an actuator value when used as an actuator parameter.

Likewise:

```text
C4
```

is a keyboard value when used as a keyboard location.

## 5. Actuator Values

Actuator values identify the physical actuator performing an action.

The language uses:

```text
LH
RH
L1
L2
L3
L4
L5
R1
R2
R3
R4
R5
LF
RF
```

Pedal identifiers include:

```text
P1
P2
P3
```

The complete actuator model is defined in `01-000_Actuators.md`.

## 6. Keyboard Values

Keyboard values identify piano keys.

White keys use a note letter and octave:

```text
C4
D4
E4
F4
G4
A4
B4
```

Black keys use the language's lowercase keyboard notation.

Relative transformations may modify keyboard values without requiring a new absolute value.

The complete keyboard-value model is defined in `01-001_Keyboard.md`.

## 7. Timing Values

Timing values describe durations and intervals.

The reference timing declaration is:

```c
const time = 1''
```

Examples include:

```text
.
2.
4.
.2
.4
.8
.16
.32
```

Timing values are interpreted according to the timing specification.

## 8. Inherited Values

The dot:

```text
.
```

represents inheritance from the surrounding context where the parameter supports inheritance.

For example:

```c
press(R1, C4, ., soft, .);
```

The `duration` and `interval` values are inherited from the current context.

Inheritance avoids repeating values that have already been supplied by an enclosing construct.

## 9. Inheritance Is Positional

Inheritance applies to the corresponding parameter position.

If an enclosing construct supplies:

```text
what
where
duration
how
interval
```

then an inner instruction can use `.` in one or more positions to inherit those corresponding values.

For example:

```c
press(., ., ., ., .);
```

means that each argument is inherited from its corresponding surrounding value, where inheritance is valid.

Inheritance does not mean that every dot refers to one universal value. Each dot refers to the value associated with its own parameter position and context.

## 10. Relative Values

A value may be transformed relative to its current value.

Examples include:

```text
.[+1]
.[+2]
[-1].
```

Relative transformations are especially important inside repeated physical patterns.

A transformation operates on the relevant component of the value.

For example, finger progression can be represented conceptually as:

```text
R1 → R2 → R3
```

and keyboard progression as:

```text
C4 → D4 → E4
```

The exact transformation rules are defined in `02-002_RelativeTransformations.md`.

## 11. Value Components

Some values contain multiple components.

For example, a finger value contains a hand component and a finger-number component:

```text
R1
```

contains:

```text
R
1
```

A keyboard value contains a key component and an octave component:

```text
C4
```

contains:

```text
C
4
```

Relative transformations can target the appropriate component without requiring the complete value to be rewritten.

## 12. Parameter Inheritance in Functions

Function parameters establish local values inside the function body.

For example:

```c
function pattern(what, where, duration, how, interval) {
    press(what, where, duration, how, interval);
};
```

The values supplied to the function call become the corresponding local parameter values.

The function body may then pass those values directly, inherit them where supported, or transform them.

## 13. Locality

Parameter values are local to the construct in which they are defined.

A transformation of a parameter produces a transformed value for the expression using that transformation. It does not silently modify the original value.

For example, using:

```text
.[+1]
```

should not permanently change the value represented by the parent context.

This allows one parent value to be used by several children with different transformations.

## 14. Shadowing

A nested construct may provide a new value for a parameter position.

The new local value takes precedence within that nested context.

The parent value remains available outside the nested scope.

This allows a general pattern to establish defaults while a nested operation specializes individual parameters.

## 15. Explicit Values and Inherited Values

A parameter may receive either an explicit value or an inherited value when the construct allows it.

For example:

```c
press(R1, C4, ., normal, .);
```

contains explicit values for actuator, key, and execution mode, while duration and interval are inherited.

This combination allows source code to state only the information that changes at each level.

## 16. Function Parameters and Call Arguments

A function declaration introduces parameters:

```c
function chord(what, where, duration, how, interval) {
    ...
};
```

A function call supplies arguments in the same order:

```text
what, where, duration, how, interval
```

The argument in each position becomes the corresponding parameter value inside the function.

A function should not depend on the names used by the caller. Parameter binding is positional.

## 17. Additional Parameters

A construct may require parameters beyond the general five execution parameters.

For example, repetition may add:

```text
count
```

after:

```text
what, where, duration, how, interval
```

Conceptually:

```text
repeat(what, where, duration, how, interval, count)
```

Additional parameters must be defined explicitly by the construct.

## 18. Missing and Empty Values

A parameter position may use `.` when the value should be inherited.

An omitted argument and an inherited argument are not automatically equivalent.

Where the grammar requires a complete argument list, an explicit `.` should be used to request inheritance.

This keeps inheritance visible in source code.

## 19. Type and Context

A value's validity depends on the parameter receiving it.

For example, a keyboard parameter should receive a valid keyboard value, while an actuator parameter should receive a valid actuator value.

The language should detect invalid combinations rather than silently converting unrelated values.

The detailed validation rules are defined by the specifications for the corresponding value types and constructs.

## 20. Composition

Values should compose naturally with functions, inheritance, repetition, and relative transformation.

A higher-level pattern can establish values, while nested constructs modify only the values that need to change.

For example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .);
};
```

Here the repeated structure can reuse the surrounding values while transforming the relevant values for the following action.

The precise execution behavior of `repeat` is defined in `03-003_Repeat.md`.

## 21. Design Principle

Values and parameters should support the central design goals of the language:

- avoid unnecessary repetition;
- preserve physical meaning;
- make transformations explicit;
- keep functions reusable;
- make nested patterns readable;
- and allow complex movements to be constructed from simple values.

## 22. Summary

The value and parameter system is based on:

- positional parameters;
- explicit values;
- contextual inheritance through `.`;
- relative transformation;
- local function parameters;
- non-mutating value transformation;
- and construct-specific additional parameters.

These mechanisms provide the foundation for expressions, inheritance, relative transformations, functions, statements, and execution.
