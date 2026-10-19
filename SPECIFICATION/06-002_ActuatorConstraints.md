# 06-002 Actuator Constraints

## 1. Overview
This document specifies the physical and logical constraints that govern the behavior of each actuator within the Piano Programming Language. Since the language adopts a physical perspective, every actuator (finger or pedal) is modeled as a real mechanical entity with inherent limitations. These constraints define the boundaries within which `duration`, `interval`, and concurrency must operate.

## 2. Physical Constraints
Each actuator is a physical object subject to the laws of mechanics. The engine must respect these limits when scheduling actions.

### 2.1. Activation Time
The **minimum activation time** ($t_{min}$) is the shortest period for which an actuator can reliably maintain a state (e.g., the minimum time a key can remain depressed). Any `duration` shorter than $t_{min}$ is physically unachievable and must be clamped or rejected.

### 2.2. Reset Time
The **reset time** ($t_{reset}$) is the mechanical time required for an actuator to return to its neutral (rest) state after a completed action. This directly constrains the minimum possible `interval` between two consecutive actions of the same actuator.

$$ \text{interval}_{min} \geq t_{reset} $$

### 2.3. Concurrent State Limitation
An actuator can exist in exactly **one state at any given moment**. It cannot simultaneously be "pressed" and "released," nor can it be assigned two different `where` targets at the same absolute time $T$. Attempting to do so constitutes a temporal conflict (see `06-001_Concurrency.md`, §3.2).

## 3. Logical Constraints
In addition to physical limits, the language enforces logical rules derived from the parameter model.

### 3.1. Parameter Binding
An actuator is defined by a fixed physical position (its `where`-independent identity). A finger cannot change its fundamental identity during execution; it can only change *which target* it acts upon, subject to reachability constraints.

### 3.2. Reachability
Each actuator (finger) has a **physical reach** — the set of keys it can plausibly target. A command that targets a note outside the actuator's reach is invalid and must be flagged by the engine.

### 3.3. The `duration`/`interval` Bound
For any sequential pair of actions on the same actuator, the total cycle time must satisfy:
$$ T_{start}(B) \geq T_{end}(A) + t_{reset}(A) $$

This enforces that an actuator never begins a new action before it has physically returned to rest.

## 4. Constraint Violation Handling
When a constraint is violated, the execution engine must not silently continue. The behavior is:
1. **Clamping:** Non-critical violations (e.g., a marginally short `duration`) are adjusted to the nearest physically valid value.
2. **Rejection with Error:** Fatal violations (e.g., unreachable target or concurrent-state conflict) are rejected, and an error is raised (see `08-000_ErrorHandling.md`).

## 5. References
- `01-000_Actuators.md` — Definition of the actuator entity.
- `05-001_Duration.md` — Lifecycle constraints on the "On" state.
- `05-002_Interval.md` — Reset-time bound on the "Off" state.
- `06-001_Concurrency.md` — State-conflict resolution in parallel execution.
- `08-000_ErrorHandling.md` — Reporting of constraint violations.
