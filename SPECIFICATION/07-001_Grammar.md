# 07-001 Grammar

## 1. Overview
This document formally defines the syntax and grammar of the Piano Programming Language using Extended Backus-Naur Form (EBNF). It establishes the precise syntactic rules for parsing programs, statements, function declarations, and expressions.

## 2. Lexical Conventions
- Identifiers: Alphanumeric strings starting with a letter (e.g., actuator IDs like R1, L1).
- Notes: Pitch representations following Excel R1C1 logic (e.g., C4, R1).
- Literals: Time values (e.g., 10ms, 1s) and qualitative parameters (e.g., staccato, legato).
- Delimiters: Parentheses ( ), Commas ,, Semicolon ;, and Dots ..

## 3. EBNF Grammar Specification

(Program Structure)
program         ::= { statement } ;

statement       ::= ( core_command | function_call ) ";" ;

(Core Commands)
core_command    ::= press_cmd | together_cmd | repeat_cmd ;

press_cmd       ::= "press" "(" what "," where "," duration "," how "," interval ")" ;
together_cmd    ::= "together" "(" argument_list ")" ;
repeat_cmd      ::= "repeat" "(" count "," what "," where "," duration "," how "," interval ")" ;

(Function Definition & Call)
function_def    ::= "def" identifier "(" parameter_list ")" "{" { statement } "}" ;
function_call   ::= identifier "(" argument_list ")" ;

parameter_list  ::= parameter { "," parameter } ;
parameter       ::= "." [ transformation ] ;

argument_list   ::= argument { "," argument } ;
argument        ::= identifier | note | time_literal | qualitative_literal ;

(Transformations)
transformation  ::= "[" operator value "]" ;
operator        ::= "+" | "-" ;
value           ::= digit { digit } ;

(Terminals and Literals)
identifier      ::= letter { letter | digit } ;
note            ::= letter [ accidental ] digit ;
time_literal    ::= digit { digit } ( "ms" | "s" ) ;
qualitative_literal ::= "staccato" | "legato" | "accent" | "normal" ;

letter          ::= "A" | ... | "Z" | "a" | ... | "z" ;
digit           ::= "0" | ... | "9" ;

## 4. Syntactic Rules & Constraints
1. Semicolon Termination: Every statement (command or function call) must be terminated with a semicolon (;).
2. Positional Parameter Enforcement: The exact positional order (what, where, duration, how, interval) must be strictly maintained across all commands and signatures.
3. Dot Notation: In function definitions, parameters must begin with a dot (.), optionally followed by a relative transformation.

## 5. References
- 00-003_LexicalStructure.md — Token definitions.
- 04-001_Parameters.md — Positional argument definitions.
- 07-000_SyntaxReference.md — High-level syntax overview.

