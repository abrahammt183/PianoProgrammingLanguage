
### فایل اول: 07-002_Examples.md
```markdown
# 07-002 Examples

This document provides practical examples of the Piano Programming Language, ranging from basic movements to complex musical patterns.

## 1. Basic Finger Movement
A simple press of a single note.
press(R1, C4, 500ms, normal, 0ms);

## 2. Chord Execution (Together)
Playing multiple notes simultaneously to form a chord.
together(
press(R1, C4, 500ms, normal, 0ms),
press(R2, E4, 500ms, normal, 0ms),
press(R3, G4, 500ms, normal, 0ms)
);

## 3. Rhythmic Repetition
Repeating a note with a specific interval.
repeat(4, R1, C4, 200ms, normal, 250ms);

## 4. Complex Pattern (Based on Clementine)
Example derived from musical sequences:
together(
press(R1, C4, 100ms, staccato, 0ms),
press(R2, G4, 100ms, staccato, 0ms)
);
repeat(2, R1, E4, 150ms, accent, 300ms);
press(R3, G4, 500ms, legato, 0ms);
