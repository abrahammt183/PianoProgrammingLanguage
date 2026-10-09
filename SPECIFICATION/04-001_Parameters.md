# 04-001 Parameters

**Date:** 2026-10-09

## Table of Contents

1. Fixed Scope and Order of Parameters
2. Meaning of the Five Core Parameters
3. Parameter Inheritance and Transformations in Function Definitions
4. Parameter Counts and Structural Examples
5. References

## 1. Fixed Scope and Order of Parameters

This document defines the order and role of operational parameters within the Piano Programming Language. The language formalizes execution from the perspective of the performer's physical actions: which finger moves, where the movement occurs, how long it lasts, the manner of execution, and the time interval until the next event.

The order of the five core parameters in all functional constructs is mandatory; any reordering or shifting is prohibited:
```text
what, where, duration, how, interval
This sequence is identical to the press command and shared five-parameter structures; together also employs this exact order.

In function definitions, it is prohibited to replace these positional slots with arbitrary names such as finger or note. The dot (.) notation is used in function signatures to represent these positional input slots. Consequently, a standard function contains exactly five slots, and the only permitted six-parameter structure is the one that appends a count parameter specifically for repeat. This document does not define the specific data types for these parameters.

2. Meaning of the Five Core Parameters

Position	Parameter	Meaning
1	what	The actuator or finger responsible for the action (the “what” that moves).
2	where	The target object or note where the action occurs (the “where” of the movement).
3	duration	The time the action remains active; for a key press, the interval from initial pressure to release.
4	how	The manner or quality of execution. The name of this parameter alone does not define a specific data type, value range, or granular physical meaning.
5	interval	The time elapsed from the start of the current event to the start of the next event. This is distinct from duration.
Therefore, what refers to the executor, not the note; and where refers to the location of execution, not the finger.

In the temporal model, comparing duration and interval clarifies the relationship between two events:

If duration > interval, the two events overlap.
If interval > duration, there is a silence/gap between them.
If duration == interval, the events occur consecutively without a gap.
This relationship clarifies the meaning of the two parameters without altering their positional order.

3. Parameter Inheritance and Transformations in Function Definitions
The dot (.) is used in this language for positional inheritance: a dot inherits its corresponding value from the surrounding context. Examples of inheritance in the specifications show that certain values can be explicitly defined while other positions are inherited from the context.

In function definitions, a dot must be written for every parameter. When necessary, a relative transformation can be applied to the input position within the function definition itself.

Example:

c
function quick_tap(., .[+1], ., ., .) {
press(., ., ., ., .);
};
In this example, the second position in the function definition is specified with the relative transformation [+1]. This transformation is part of the function’s definition mapping, not a change applied by the caller to the argument during invocation.

Transformation symbols and their locations within an expression identify the component being modified. Specifications provide examples such as [+1]. and .[+1] for addressing components.

Function calls provide only the values for the five positions in the required order:

c
quick_tap(R2, C4, 10, normal, 10);
In this instance, the relative transformation defined in the function signature is applied during the execution of the function. This example demonstrates the conceptual location of the transformation definition; precise evaluation rules and valid ranges for transformations must be consulted in the specific rules for inheritance and relative transformations, as this document alone does not establish new rules.

4. Parameter Counts and Structural Examples
Functions defined in this language must adhere to a fixed parameter structure:

The five core parameters; or
In the case of repeat, a six-parameter structure that appends a count parameter.
For five-parameter definitions, five dots must be used; for six-parameter definitions, six dots must be used. The repeat command places count after the five core parameters.

Five-Parameter Function Example
c
function quick_tap(., ., ., ., .) {
press(., ., ., ., .);
};
Six-Parameter Function Example (Consistent with repeat)
c
function burst(., ., ., ., ., .) {
repeat(., ., ., ., ., .);
};
In the second structure, the sixth dot corresponds to count. Therefore, count is not a general additional parameter for every function, nor does it shift or remove the primary five-parameter order.

The examples provided in this section are for illustrating the count and order of positions. They do not define:

Data types;
Default values;
The range of count;
Complete argument matching rules.
These elements are only determinable if explicitly specified in their respective functional specifications.

5. References
Sources used in this document:

00-004_ValuesAndParameters.md — Order and position of parameters.
03-001_Press.md — Signature and parameters of press.
03-002_Together.md — Five-parameter structure of together.
03-003_Repeat.md — Position of the count parameter.
04-000_Functions.md — Function structure.
02-001_Inheritance.md — Inheritance via the dot operator.
02-002_RelativeTransformations.md — Relative transformations.
README.md — Introduction to the language’s physical approach.
