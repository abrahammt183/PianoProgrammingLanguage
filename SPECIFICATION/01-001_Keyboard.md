# Keyboard

## 1. Overview

The keyboard is the physical set of piano keys addressed by the Piano Programming Language.

A keyboard value identifies a specific key by its musical letter and octave. Keyboard values are used primarily as the `where` parameter of physical actions.

The language uses a compact, direct notation so that a pianist can read keyboard locations without translating them from staff notation.

## 2. White Keys

White keys are represented by an uppercase key letter followed by an octave number.

The seven white-key letters are:

```text
C D E F G A B
```

Examples:

```text
C4
D4
E4
F4
G4
A4
B4
```

`C4` therefore identifies the C key in octave 4.

## 3. Octaves

The number following a white-key letter identifies the octave.

For example:

```text
C3
C4
C5
```

identify three different C keys.

The octave is part of the keyboard value and can be transformed independently from the key letter.

## 4. Absolute Keyboard Values

A complete keyboard location can be written directly:

```c
press(R1, C4, ., normal, .);
```

Here `R1` identifies the actuator and `C4` identifies the keyboard location.

## 5. Relative Keyboard Values

Keyboard values can be transformed relative to their current value.

The language supports transformations of the key-letter component and the octave component separately.

For example:

```text
C[+1]4
```

changes the key letter while keeping the octave unchanged.

Thus:

```text
C[+1]4
```

represents `D4`.

Similarly:

```text
C4[+1]
```

changes the octave while keeping the key letter unchanged.

Thus:

```text
C4[+1]
```

represents `C5`.

## 6. Fractional Key Transformations

The key-letter component can also use fractional relative transformations.

For example:

```text
C[+1.5]4
```

represents a black key between the corresponding white-key positions according to the language's keyboard notation.

## 7. Black Keys

Black keys are represented using a lowercase letter based on the next white key.

Examples include:

```text
b0
d1
f1
a1
```

The lowercase notation distinguishes black-key values from the uppercase notation used for white keys.

## 8. Component Structure

A white-key value has two principal components:

```text
key letter + octave
```

For example:

```text
C4
```

contains:

```text
C
4
```

Relative transformations can target either component.

Examples:

```text
C[+1]4
C4[+1]
```

The first changes the key letter and the second changes the octave.

## 9. Inheritance

The symbol `.` can inherit a keyboard value from the surrounding context when the parameter position supports inheritance.

For example:

```c
press(., ., ., ., .);
```

can inherit the keyboard location from the surrounding context.

Inheritance avoids repeating the same key location when several nested actions operate on the same value.

## 10. Keyboard Values in `press()`

The keyboard location is supplied as the second parameter of `press()`:

```c
press(what, where, duration, how, interval);
```

Example:

```c
press(R1, C4, ., normal, .);
```

Here `C4` is the `where` value and identifies the key to be pressed.

## 11. Keyboard Values in Repeated Patterns

Relative keyboard addressing can be used to describe related repeated actions.

Example:

```c
repeat(R1, C4, ., R1, .2, 4) {
    press(., ., ., ., .);
    press(.[+1], [+1]., ., ., .)
};
```

Within this context, the keyboard value supplied by the repeat construct can be transformed relative to the current value.

Relative addressing is intended for describing relationships inside repeated structures rather than replacing clear absolute keyboard values in ordinary examples.

## 12. Relationship to Actuators

A keyboard value identifies **where** an action occurs.

An actuator identifies **what** performs it.

For example:

```c
press(R1, C4, ., normal, .);
```

can be read physically as:

```text
what:  R1
where: C4
```

The first identifies the finger and the second identifies the key.

Keeping these concepts separate allows the same keyboard location to be played by different actuators and the same actuator to address different keyboard locations.

## 13. Keyboard Values and Timing

Keyboard values describe spatial location only. They do not contain duration or interval information.

For example:

```text
C4
```

identifies a key, while:

```text
.
2.
.2
```

are timing values.

The separation of keyboard and timing values allows the same physical location to participate in patterns with different timing.

## 14. Summary

Keyboard values provide the language's notation for piano-key locations.

The basic forms are:

```text
White keys:
C4
D4
E4
F4
G4
A4
B4

Relative key transformation:
C[+1]4

Relative octave transformation:
C4[+1]

Fractional key transformation:
C[+1.5]4

Black keys:
b0
d1
f1
a1
```

A keyboard value describes **where** a physical action occurs. It can be written absolutely, inherited with `.`, or transformed relatively where the language permits.
