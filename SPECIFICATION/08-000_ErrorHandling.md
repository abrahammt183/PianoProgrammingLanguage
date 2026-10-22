# 08-000 Error Handling

This document outlines the error categories and the behavior of the interpreter when encountering invalid syntax or physical impossibilities.

## 1. Syntax Errors
These errors occur during the parsing phase before execution begins.

### 1.1 Missing Delimiters
Occurs when a semicolon (;) is missing at the end of a statement or a parenthesis is not closed.
- Error Code: `ERR_SYNTAX_DELIMITER`

### 1.2 Invalid Parameter Count
Occurs when a core command is provided with more or fewer than the required 5 parameters.
- Error Code: `ERR_SYNTAX_ARG_COUNT`

## 2. Logical & Runtime Errors
These errors are detected during the execution phase.

### 2.1 Actuator Conflict
Occurs when two simultaneous commands (within a `together` block) attempt to move the same actuator to different locations at the same time.
- Error Code: `ERR_RUNTIME_ACTUATOR_CONFLICT`

### 2.2 Physical Constraint Violation
Occurs when a command requests a speed or a duration that is physically impossible for the hardware (e.g., a duration shorter than the minimum actuator reset time).
- Error Code: `ERR_RUNTIME_CONSTRAINT_VIOLATION`

## 3. Error Reporting
Upon encountering an error, the interpreter will:
1. Halt the current execution thread.
2. Return the error code and the line number where the error was detected.
3. (Optional) Attempt to recover if the error is contained within a non-critical `together` block, depending on the implementation mode.
