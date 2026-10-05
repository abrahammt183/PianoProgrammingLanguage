# Introduction

## 1. Purpose

Piano Programming Language defines a textual notation for describing piano performance as physical actions.

The language is designed for pianists, piano learners, composers, researchers, and software developers who need a representation closer to the movements of the performer than conventional staff notation provides.

## 2. Scope

The language describes:

* hands;
* individual fingers;
* feet;
* pedals;
* piano keys;
* timing;
* duration;
* intervals between actions;
* dynamics;
* articulation;
* simultaneous actions;
* repeated actions;
* reusable functions;
* inherited parameters;
* and relative transformations.

The language may represent conventional musical passages, technical exercises, finger patterns, and physical performance instructions.

## 3. Relationship to Traditional Notation

Traditional notation and Piano Programming Language describe different aspects of a performance.

Traditional notation describes musical information such as pitch, rhythm, harmony, dynamics, and phrasing.

Piano Programming Language describes executable performer actions.

A performance may therefore be represented at multiple levels:

```text
Musical composition
        ↓
Musical interpretation
        ↓
Piano Programming Language
        ↓
Physical piano actions
        ↓
Sound
```

The language is not restricted to reproducing traditional notation. It may express physical relationships that are difficult to represent directly on a musical staff.

## 4. Core Principle

A piano instruction should be expressible in terms of the smallest meaningful physical action.

The fundamental action is:

```c
press(what, where, duration, how, interval);
```

The five parameters describe:

1. **what** — the actuator performing the action;
2. **where** — the target key or physical location;
3. **duration** — how long the action remains active;
4. **how** — the manner of execution;
5. **interval** — the time until the next instruction begins.

## 5. Design Direction

The language favors:

* positional parameters;
* inheritance through `.`;
* relative transformations through `[+n]` and `[-n]`;
* reusable functions;
* explicit synchronization;
* asynchronous actions;
* and concise C-style syntax.

The language should remain understandable without requiring an interpreter or graphical notation editor.

