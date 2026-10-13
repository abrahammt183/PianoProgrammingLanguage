# 05-002 Interval

## 1. Overview
In the Piano Programming Language, `interval` is a temporal parameter that defines the waiting period between the conclusion of one physical event and the initiation of the next. While `duration` defines the lifespan of an active state, `interval` defines the silence or the gap between states.

## 2. Temporal Mechanics
The timing of a sequence is calculated as a cumulative progression. The "next" event's start time ($T_{start\_next}$) is determined by the current event's end time ($T_{end\_current}$) plus the specified `interval`.

### 2.1. The Calculation Formula
For any two sequential actions $A$ and $B$ performed by the same actuator:
$$T_{start}(B) = T_{end}(A) + \text{interval}(A, B)$$

Where:
- $T_{end}(A) = T_{start}(A) + \text{duration}(A)$

## 3. Relationship with Duration
It is vital to distinguish between `duration` and `interval`:
- **Duration:** The "On" time (the period the key is depressed).
- **Interval:** The "Off" time (the period between the key being released and the next press).

A sequence with `duration = 100ms` and `interval = 50ms` results in a rhythmic pattern of $150\text{ms}$ per cycle (100ms active + 50ms inactive).

## 4. Deterministic Gap
The `interval` parameter is strictly non-numeric in its logical application within functional definitions but accepts `Time` units (e.g., `10ms`, `1s`) as values. Like all temporal parameters, the `interval` is subject to the physical constraints of the actuator (e.g., a piano hammer cannot reset faster than its mechanical limit).

## 5. Edge Cases
### 5.1. Zero Interval
An `interval` of $0$ implies that the next action begins the exact millisecond the previous action concludes. This is the minimum possible gap between two sequential physical movements.

### 5.2. Overlapping Intervals (Concurrency)
In cases of parallel execution (different actuators), the `interval` of one actuator does not block or affect the `interval` of another. Each actuator maintains its own independent timeline of "On" and "Off" states.

## 6. References
- `05-001_Duration.md` — Lifecycle of an active event.
- `06-001_Concurrency.md` — Independent timelines in parallel execution.
