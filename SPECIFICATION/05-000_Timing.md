# 05-000 Timing

## 1. Overview
This chapter establishes the temporal model of the Piano Programming Language. Timing in this language is not treated as a global metronome or a continuous stream of beats, but as a collection of relative temporal constraints defined by the individual physical actions of the actuators.

## 2. Temporal Philosophy
The language adopts a physical perspective on time. Time is defined by:
1. **The physical duration of an event:** How long an actuator remains in a specific state (e.g., how long a key is held).
2. **The interval between events:** The time elapsed from the initiation of one action to the initiation of the next.

These two parameters are independent but complementary. Their relationship defines the rhythmic and expressive structure of a performance.

## 3. Core Principles
The temporal model is governed by three foundational rules:

### 3.1. Independence of Actions
Each actuator operates on its own timeline. Concurrency is handled by the execution model, where multiple `press` or `together` statements can exist in parallel without requiring a shared global clock.

### 3.2. Localized Control
Temporal parameters are defined locally within statements. There are no global tempo or BPM (Beats Per Minute) commands in the core language; timing is strictly calculated relative to the previous action of the specific actuator involved.

### 3.3. Determinism
Given a fixed sequence of physical inputs and their associated `duration` and `interval` values, the execution is entirely deterministic. The resulting timing is a direct consequence of the sum of these local temporal constraints.

## 4. Chapter Scope
The following sections in this chapter will define:
- `05-001_Duration.md`: Rules for defining the active time of an event.
- `05-002_Interval.md`: Rules for calculating the time gap between sequential events.

## 5. References
- `00-002_DesignPrinciples.md` — Physical perspective on performance.
- `03-001_Press.md` — The role of temporal parameters in core statements.
- `06-001_Concurrency.md` — Interaction of temporal models in parallel execution.
