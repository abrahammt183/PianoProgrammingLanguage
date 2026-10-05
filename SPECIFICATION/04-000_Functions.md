# 04-000 Functions

## 1. Overview

Functions provide reusable musical and physical patterns in Piano Programming Language.

A function defines a named operation once and allows that operation to be used repeatedly with different values. Functions are one of the main mechanisms for moving from primitive instructions toward higher levels of abstraction.

The language follows the principle:

> Learn from the bottom up; play from the top down.

At a low abstraction level, a player can write individual `press()` operations. At a higher level, those operations can be grouped into functions representing reusable playing patterns.

Functions work together with parameters, inheritance, relative transformations, nesting, asynchronous actions, `together()`, and `repeat()`.

## 2. Function Definition

A function is defined with the `function` keyword:

```c
function name(what, where, duration, how, interval) {
    // function body
};
```

The function name identifies the reusable operation.

The parameter list follows the standard parameter order:

```text
what, where, duration, how, interval
```

This order is preserved for every function.

For example:

```c
function note(what, where, duration, how, interval) {
    press(what, where, duration, how, interval);
};
```

A function body contains the instructions executed when the function is called.

## 3. Function Parameters

Function parameters provide the values that control an invocation of the function.

| Parameter | Purpose |
|---|---|
| `what` | actuator or other physical performer |
| `where` | keyboard key or other target location |
| `duration` | how long the action remains active |
| `how` | mode or manner of performing the action |
| `interval` | time until the next instruction |

The same parameter positions are used by primitive operations and functions.

A function can receive values from its caller and pass them to operations in its body.

## 4. Function Parameters as Transformable Values

Function parameters are not limited to being passed unchanged.

A function may transform an incoming parameter before using it.

For example:

```c
function note(.[+1], [+1]., [+1], ., [-1]) {
    press(., ., ., ., .);
};
```

Here the function definition transforms the values supplied by the caller:

- `what` is transformed by `.[+1]`
- `where` is transformed by `[+1].`
- `duration` is transformed by `[+1]`
- `how` is inherited with `.`
- `interval` is transformed by `[-1]`

The transformed values are then available inside the function body.

This allows a function to describe a general relationship rather than one fixed physical action.

## 5. Inheritance in Functions

A period (`.`) means that the corresponding value is inherited.

For example:

```c
function note(., ., ., ., .) {
    press(., ., ., ., .);
};
```

When the function is called, each `.` receives the corresponding value from the calling context.

Inheritance allows a function to remain general while still operating on the values supplied by its caller.

## 6. Local Transformation

Transformations performed by a function are local to that function invocation.

A child function may modify an inherited parameter without modifying the corresponding value in its parent scope.

Conceptually:

```text
parent value
     |
     v
 child function
     |
     +-- transformed local value
     |
     v
parent value remains unchanged
```

If a function receives `C4` and transforms its `where` parameter to the next key, the transformation applies inside that function's scope.

After the function finishes, the caller's original value remains unchanged.

This permits functions to be composed without unintended changes propagating back through their callers.

## 7. Function Scope

Each function invocation has its own local parameter context.

When a function is called:

1. The caller supplies or inherits values.
2. Those values establish the function's parameter context.
3. Transformations are applied within that context.
4. The function body executes using the resulting values.
5. The caller's parameter context remains unchanged.

This behavior is especially important for nested functions.

A function can therefore build on another function while making local changes to its parameters.

## 8. Function Bodies

A function body may contain valid operations allowed by the language.

For example:

```c
function note(what, where, duration, how, interval) {
    press(what, where, duration, how, interval);
};
```

A function can also contain a group of simultaneous actions:

```c
function chord(what, where, duration, how, interval) {
    together(what, where, duration, how, interval) {
        // simultaneous actions
    };
};
```

A function may therefore represent either a primitive-like operation or a more complex musical structure.

## 9. Nesting Functions

Functions may use other functions.

For example:

```c
function note(., ., ., ., .) {
    press(., ., ., ., .);
};

function chord(., ., ., ., .) {
    together() {
        note(., ., ., ., .);
        note(.[+2], [+2]., ., ., .);
        note(.[+4], [+4]., ., ., .);
    };
};
```

The outer function establishes a higher-level structure while the inner function provides the reusable lower-level operation.

This is one of the central abstraction mechanisms of Piano Programming Language.

## 10. Functions and Abstraction

Functions allow the same physical pattern to be expressed at different abstraction levels.

For example, the primitive level may describe:

```c
press(R1, C4, ., normal, .);
```

A function can give this action a meaningful reusable structure:

```c
function note(., ., ., ., .) {
    press(., ., ., ., .);
};
```

A higher-level function can then use `note()` as part of a larger pattern.

The resulting hierarchy can be understood as:

```text
primitive action
    |
    v
reusable operation
    |
    v
musical pattern
    |
    v
larger musical structure
```

Functions therefore support the language's goal of matching the way a player can think about increasingly complex physical patterns.

## 11. Functions and Relative Transformations

Relative transformations are useful when a function describes a pattern rather than one fixed set of values.

For example:

```c
function chord(., ., ., ., .) {
    together() {
        note(., ., ., ., .);
        note(.[+2], [+2]., ., ., .);
        note(.[+4], [+4]., ., ., .);
    };
};
```

The first note uses the inherited values.

The second and third notes transform the corresponding actuator and keyboard values.

Relative addressing is otherwise restricted according to the language rules; functions may use transformations as part of their parameter definitions and nested calls where the language permits them.

## 12. Functions and `together()`

A function can contain `together()` when several actions must begin simultaneously.

For example:

```c
function chord(., ., ., ., .) {
    together() {
        note(., ., ., ., .);
        note(.[+2], [+2]., ., ., .);
        note(.[+4], [+4]., ., ., .);
    };
};
```

The function represents the chord as one reusable structure while `together()` defines the simultaneous execution of its component actions.

The execution behavior of `together()` is defined in `03-002_Together.md`.

## 13. Functions and `repeat()`

A function may also contain or be used with `repeat()`.

This allows repeated physical or musical patterns to be encapsulated as reusable operations.

For example:

```c
function repeatedPattern(what, where, duration, how, interval) {
    repeat(what, where, duration, how, interval, 4) {
        press(., ., ., ., .);
    };
};
```

The function provides the reusable pattern, while `repeat()` controls its repetition.

The detailed semantics of repetition are defined in `03-003_Repeat.md`.

## 14. Function Calls

Defining a function does not execute it.

A function executes when it is called.

Function-call syntax and argument rules are specified in `04-002_FunctionCalls.md`.

The parameter semantics used by calls are specified in `04-001_Parameters.md`.

## 15. Function Composition

Functions can be composed to construct larger patterns from smaller ones.

A useful structure is:

```text
press()
  |
  v
note()
  |
  v
chord()
  |
  v
larger musical pattern
```

Each layer can hide implementation details from the layer above it.

This allows a player to work at the level of the pattern currently being played instead of repeatedly describing every primitive action.

## 16. Naming Functions

Function names identify reusable operations.

Names should describe the operation or musical pattern represented by the function.

For example:

```c
function note(...) { ... }
function chord(...) { ... }
function repeatedPattern(...) { ... }
```

The language specification does not require a function name to correspond to a traditional musical term. A function may represent any reusable physical or musical behavior expressible by the language.

## 17. Function Definition vs. Function Execution

A function definition describes behavior.

A function call requests that behavior.

These are separate concepts:

```text
definition
    |
    | describes
    v
behavior
    ^
    | requests
    |
call
```

Defining a function therefore has no immediate physical effect on the actuators.

Physical execution occurs only when the function is invoked as part of program execution.

## 18. Summary

Functions provide reusable abstraction in Piano Programming Language.

The essential properties are:

1. Functions are defined with `function`.
2. Every function follows the standard parameter order:
   `what, where, duration, how, interval`.
3. Parameters can be inherited with `.`.
4. Parameters can be transformed where permitted.
5. Transformations are local to the function invocation.
6. A child function does not modify the parent's parameter context.
7. Functions may contain other language constructs.
8. Functions may call other functions.
9. Functions can use `together()` and `repeat()`.
10. Functions allow physical playing patterns to be represented at progressively higher abstraction levels.
11. A function definition describes behavior; a function call executes it.

Functions are therefore a central mechanism for turning primitive physical instructions into reusable musical structures.
