# Piano Programming Language

Piano Programming Language is a notation and programming language for describing piano performance from the pianist's physical perspective.

Traditional music notation primarily describes musical structure and sound. This language describes the physical actions that produce that sound:

* which hand or finger acts;
* which key is pressed;
* how long the key remains active;
* how the action is performed;
* when the next action begins;
* and how multiple actions relate to one another.

The language is intended to describe ordinary musical notation while also expressing physical piano techniques that are difficult or inconvenient to represent directly in traditional notation.

## Philosophy

> Learn from the bottom up; play from the top down.

The language treats a piano performance as a sequence of executable physical instructions. Musical ideas can be represented as reusable functions, parameterized gestures, transformations, and synchronized actions.

A simple and elegant design is preferred over unnecessary complexity.

## Example

```c
together(R1, C4, ., soft, .) {
    press(., ., 4., ., .);
    press(.[+1], [+1]., 3., ., .);
    press(.[+2], [+2]., 2., ., .);
    press(.[+3], [+3]., ., ., .);
    press(.[+4], [+4]., .2, ., .);
}
```

This describes five simultaneous actions:

| Finger | Key | Duration |
| ------ | --- | -------: |
| R1     | C4  |  4 beats |
| R2     | D4  |  3 beats |
| R3     | E4  |  2 beats |
| R4     | F4  |   1 beat |
| R5     | G4  |   ½ beat |

The example demonstrates parameter inheritance, relative transformation, synchronization, and independent durations.

## Design Goals

1. Describe real piano performance precisely.
2. Remain readable by humans.
3. Support traditional musical structures.
4. Express physical actions directly.
5. Support reusable and parameterized patterns.
6. Provide concise relative notation.
7. Preserve a clear relationship between notation and physical movement.
8. Remain suitable for future implementation.

## Project Status

The project is currently in the specification phase.

The language specification will be established before implementation begins.

## License

To be determined.

