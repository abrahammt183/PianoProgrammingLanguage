# Relative Transformations

## 1. Overview

A relative transformation derives a value from an existing value rather than repeating an absolute value. In the Piano Programming Language, transformations make related physical movements and keyboard positions readable as changes to a shared context.

The notation uses square brackets containing a signed offset, such as `[+1]` or `[-1]`. The position of the brackets determines **which component** of a compound value changes.

This specification describes the established transformation forms. Rules not yet established—such as out-of-range behavior—are identified as open questions rather than assumed.

## 2. Context and Inheritance

The symbol `.` inherits the value of the corresponding parameter from the surrounding context. A transformation can be applied to that inherited value.

For example, inside a repeated pattern whose `what` parameter is `R1`:

```text
.        inherited R1
.[+1]    R2
```

Likewise, if the inherited `where` value is `C4`:

```text
.        inherited C4
[+1].    D4
.[+1]    C5
```

A transformation needs a value to transform. An inherited transformation therefore depends on the corresponding surrounding parameter being available.

## 3. Component Position

Relative syntax distinguishes transformations of different components.

| Value | Form -  | Component changed | Result |
| ----  | ------- | ----------------- | ------ |
| `R1`  | `.[+1]` | Finger number     | `R2`   |
| `R1`  | `[-1].` | Hand component    | `L1`   |
| `C4`  | `[+1].` | Key letter        | `D4`   |
| `C4`  | `.[+1]` | Octave            | `C5`   |

The same written form can target different components depending on the **kind of value** being transformed. The parameter context establishes whether a value is a finger, a keyboard location, or a timing value.

## 4. Finger Transformations

A finger identifier consists of a hand component and a finger-number component:

```text
R1
```

For an inherited finger value of `R1`:

```text
.[+1]    R2
.[+2]    R3
[-1].    L1
```

The suffix transformation changes the finger number; the prefix transformation changes the hand component. A hand-component change preserves the finger number in the established `R1` → `L1` example.

The language uses `L1`–`L5` and `R1`–`R5`, not `LH1` or `RH1`.

## 5. Keyboard Transformations

A white-key location consists of a key letter and an octave:

```text
C4
```

The key-letter component can change while the octave remains fixed:

```text
C[+1]4    D4
```

The octave can change while the key letter remains fixed:

```text
C4[+1]    C5
```

The corresponding inherited forms, when `where` is `C4`, are:

```text
[+1].     D4
.[+1]     C5
```

### 5.1 Fractional key offsets

The established notation also permits a fractional key transformation:

```text
C[+1.5]4
```

This denotes `d4` in the established examples. Black-key names use lowercase letters associated with the next white key. The complete mapping and any additional fractional-offset rules belong to the keyboard specification.

## 6. Timing Transformations

Timing values also support relative transformations. For timing, the established bracket notation changes duration or interval multiplicatively:

```text
[+1]    double the inherited time
[+2]    quadruple the inherited time
[-1]    halve the inherited time
[-2]    quarter the inherited time
```

For example, if the inherited time is one second, `[+1]` produces two seconds and `[-1]` produces half a second.

Timing transformations are distinct from finger-number and keyboard-component offsets: their meaning is determined by the timing parameter context. The timing specifications define the base time and duration/interval behavior.

## 7. Relative Transformations in `repeat()`

Relative addressing is used inside repeated patterns, where each action can refer to the values supplied by the surrounding repetition context.

The established example is:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

The first `press()` inherits its arguments. In the second, `.[+1]` transforms the inherited finger component and `[+1].` transforms the inherited keyboard letter. The other arguments continue to inherit their corresponding values.

The example is retained in its established form. This chapter does not introduce relative addressing into ordinary, non-repeated `press()` examples.

## 8. Transformations Do Not Redefine Their Source

A relative expression describes a derived value. It should not be read as an instruction to rename the original actuator, key, or parameter. Whether successive iterations update a repetition's current context is an execution rule and must be defined by the repetition and execution-model specifications; it is not inferred from the transformation notation alone.

## 9. Validity and Open Questions

The following details require explicit decisions elsewhere in the specification before an implementation can rely on them:

- What happens when a finger-number offset goes outside `1`–`5`.
- Which additional hand-component offsets, if any, are valid beyond the established `R` → `L` example.
- How key-letter offsets behave across octave boundaries.
- The complete treatment of fractional keyboard offsets and unavailable black keys.
- Whether a relative value in one repetition becomes the base for a later repetition.
- The precise error reported when a transformation has no inherited source or an invalid result.

These questions should not be resolved by silently wrapping, clamping, or substituting values.

## 10. Summary

Relative transformations express changes to inherited or explicitly written values without repeating the entire value. Their meaning depends on the value category and on where the bracketed offset appears:

```text
Finger:    .[+1]      next finger number
Finger:    [-1].      change hand component
Keyboard:  [+1].      next key letter
Keyboard:  .[+1]      next octave
Timing:    [+1]       double inherited time
```

They are particularly useful inside `repeat()`, where a compact pattern can describe a related sequence of physical actions.
