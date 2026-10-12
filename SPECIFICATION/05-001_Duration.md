# 05-001 Duration

## 1. Overview
The `duration` parameter defines the temporal extent of a single physical event. In the context of the Piano Programming Language, it specifies how long an actuator (finger/key) maintains its state of action from the moment of initiation until the moment of release.

## 2. Definition and Scope
`duration` is a temporal value that dictates the "hold time" of a command. It is a core parameter in all state-changing commands, such as `press`.

Mathematically, if an event $E$ starts at time $T_{start}$, the event is active until:
$$T_{end} = T_{start} + \text{duration}$$

## 3. Physical Interpretation
In a physical piano performance, `duration` represents the interval between the initial depression of the key and the lifting of the finger from the key. 

- **Zero Duration:** A `duration` of $0$ implies an instantaneous impulse. While the actuator state changes, the time spent in the "active" state is negligible.
- **Extended Duration:** Larger `duration` values result in longer sustained notes or continuous pressure.

## 4. Relationship with Interval
It is critical to distinguish `duration` from `interval`. While `interval` controls the timing of the *next* event, `duration` controls the lifecycle of the *current* event.

The relationship between `duration` ($D$) and `interval` ($I$) determines the rhythmic texture:
- **Overlapping ($D > I$):** The current event is still active when the next event begins.
- **Consecutive ($D = I$):** The current event ends exactly when the next event begins.
- **Gapped ($D < I$):** There is a period of inactivity (silence) between the end of the current event and the start of the next.

## 5. Constraints and Units
*(Note: Specific units and valid ranges for duration are defined in the core Value System and will be detailed in the upcoming Type specifications.)*

- `duration` is strictly a non-negative temporal value.
- It cannot be negative.

## 6. References
- `04-001_Parameters.md` — Positional definition of `duration`.
- `05-000_Timing.md` — Introduction to the temporal model.
- `05-002_Interval.md` — Comparison between duration and interval.
- `03-001_Press.md` — Implementation of duration in the `press` command.
