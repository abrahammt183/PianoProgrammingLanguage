# Inheritance

## 1. Overview

Inheritance allows an expression to reuse a value supplied by its surrounding context. The inheritance symbol is:

```text
.
```

The symbol is interpreted **positionally**: it inherits the value corresponding to the parameter position in which it appears. This avoids repeating the same actuator, keyboard location, duration, execution mode, or interval throughout a pattern.

## 2. Parameter Context

The general parameter order is:

```text
(what, where, duration, how, interval)
```

A construct can establish values for these positions. An expression inside that construct may use `.` to request the corresponding surrounding value.

For example:

```c
press(R1, C4, ., normal, .);
```

The actuator (`R1`), location (`C4`), and mode (`normal`) are explicit. The duration and interval are inherited from the surrounding context.

## 3. Positional Inheritance

Each occurrence of `.` is interpreted independently according to its position. It does not mean “repeat the previous argument” or “reuse the previous statement.”

For example:

```c
press(., ., ., ., .);
```

requests the surrounding values for all five parameters, in order:

| Position | Parameter | Meaning of `.` |
| --- | --- | --- |
| 1 | `what` | Inherit the actuator |
| 2 | `where` | Inherit the location |
| 3 | `duration` | Inherit the duration |
| 4 | `how` | Inherit the execution mode |
| 5 | `interval` | Inherit the interval |

An inherited value must be appropriate for its parameter position.

## 4. Explicit Values and Inherited Values

A call can mix explicit and inherited arguments:

```c
press(R1, C4, ., normal, .);
```

Writing an explicit value supplies that value at the corresponding position. Writing `.` requests the corresponding value from context. Neither choice changes the meaning of the other positions.

An omitted argument is **not** automatically equivalent to `.`. The inheritance symbol must be written where inheritance is intended.

## 5. Inheritance in Nested Constructs

A surrounding construct can provide a parameter context for expressions in its body. A nested expression may reuse that context without spelling out every value.

For example, a repeated pattern can establish its own values:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

The first `press()` requests the corresponding surrounding values. In the second `press()`, the actuator and keyboard location are expressed relative to inherited values, while the remaining positions continue to use inheritance.

The example follows the established repeat notation. The detailed meaning of relative transformations belongs to `02-002_RelativeTransformations.md`.

## 6. Inheritance and Relative Transformation

Inheritance can serve as the starting value for a relative transformation. For example:

```text
.[+1]
[-1].
```

The position of the transformation determines which component is transformed, according to the value type and the relative-transformation rules.

For a finger value such as `R1`, `.[+1]` can refer to the next finger of the same hand. For a keyboard value, `[+1].` can refer to the next key-letter position in the same octave.

These expressions are not interchangeable with a bare `.`: a bare `.` inherits without requesting a transformation.

## 7. Function Parameters

Functions can accept the language's positional parameters and pass them into actions:

```c
function pattern(what, where, duration, how, interval) {
    press(what, where, duration, how, interval);
};
```

Named parameters refer to the values received by the function. The inheritance symbol is a separate mechanism: it requests a value from the applicable surrounding parameter context rather than naming a parameter directly.

A nested construct may establish a more local value for a position. Where the language permits such shadowing, the local value takes precedence for expressions inside that context.

## 8. Inheritance Does Not Imply Mutation

Inheriting a value does not, by itself, modify the surrounding value. Likewise, expressing a transformed value does not mean that the source value has been mutated.

This separation lets a pattern reuse its context while describing variations of the same physical gesture.

## 9. Context Requirements

Inheritance requires an applicable value for the requested position. This document does not invent a default actuator, key, duration, mode, or interval when the surrounding context does not provide one.

The precise rules for unresolved inheritance, validation, and diagnostics belong to `08-000_ErrorHandling.md` and the execution-related specifications.

## 10. Summary

- `.` inherits the corresponding value from the surrounding context.
- Inheritance is positional, following `(what, where, duration, how, interval)`.
- Explicit and inherited arguments can appear in the same call.
- `.` does not mean an omitted argument or the previous argument.
- Relative transformations can be applied to inherited values where permitted.
- Nested constructs can provide more local parameter contexts.
- Inheritance alone does not mutate its source value.

Inheritance supports the language's goal of describing physical performance with minimal repetition while preserving the meaning of each parameter.
