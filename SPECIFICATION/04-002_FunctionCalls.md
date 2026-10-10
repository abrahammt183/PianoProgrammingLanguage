04-002 Function Calls
1. Overview
This document specifies the syntax and rules for invoking functions within the Piano Programming Language. Function calls bridge the gap between a high-level functional abstraction and the underlying core commands (like press or together).

2. Invocation Syntax
A function call consists of the function’s identifier followed by a comma-separated list of arguments enclosed in parentheses. The number of arguments must exactly match the number of parameter slots defined in the function’s signature.

2.1. Direct Argument Passing
When calling a function, literal values or identifiers (such as actuators or notes) are passed into the positional slots.

c
// Calling a standard 5-parameter function
function_name(arg1, arg2, arg3, arg4, arg5);
3. Interaction with Transformations
The most critical aspect of function calls is how they interact with the relative transformations (e.g., [+1]) defined during the function’s declaration.

3.1. Pre-applied Transformations
If a function definition includes a transformation in its signature, the caller does not need to (and cannot) apply that transformation again. The transformation is “baked into” the function’s logic.

Example:

If a function is defined as:

function quick_tap(., .[+1], ., ., .)

The second argument provided in the call will be transformed by [+1] before being passed to the core command.

Correct Call:

quick_tap(R1, C4, 10, staccato, 10);

(Internally, the where parameter becomes C5 because of the [+1] in the definition.)

4. Rules of Invocation
Positional Integrity: Arguments must be provided in the exact order specified by the function’s signature.
Argument Count: The number of arguments must strictly match the number of dots (.) in the function definition (5 for standard, 6 for extended).
Transformation Scope: Transformations defined in the signature apply to the result of the argument passed, not to the argument itself during the call.
No Named Arguments: The language does not support named parameters; all arguments are resolved by position.
5. References
04-001_Parameters.md — Parameter ordering and definitions.
02-001_Inheritance.md — The dot (.) operator logic.
02-002_RelativeTransformations.md — Mechanics of [+1] and similar transforms.
