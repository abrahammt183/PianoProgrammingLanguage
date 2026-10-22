# 09-000 Standard Library

The Standard Library provides a collection of pre-defined functions and macros to simplify common musical tasks. These functions use the dot notation for parameter inheritance.

## 1. Articulation Macros
These functions modify the `how` parameter of the `press` command.

### 1.1 Staccato Press
A short, detached note.
def staccato_press(., ., duration, ., .) {
press(., ., duration * 0.5, staccato, .);
}

### 1.2 Legato Press
A smooth, connected note.
def legato_press(., ., duration, ., .) {
press(., ., duration, legato, .);
}

## 2. Chord Macros
Functions to simplify playing chords.

### 2.1 Major Triad
Plays a major triad starting from the given note.
def major_triad(., where, duration, ., .) {
together(
press(R1, where, duration, normal, 0ms),
press(R2, where + 4, duration, normal, 0ms),
press(R3, where + 7, duration, normal, 0ms)
);
}

## 3. Sequence Macros
### 3.1 Arpeggio
Plays notes of a chord sequentially.
def arpeggio(., where, duration, ., .) {
press(R1, where, duration, normal, 0ms);
press(R2, where + 4, duration, normal, 0ms);
press(R3, where + 7, duration, normal, 0ms);
}

