# Producer–Consumer Exercise

The aim of this exercise is to practice concurrency, multithreading, race conditions and synchronization constructs.

This is a variant of the classical producer–consumer problem where two processes respectively put and get items from a bounded buffer, a queue. The aim is to design the system in such a way that no data is lost, no work is replicated and the processes work as efficiently as possible.

In this exercise we will have a set of producers. They will generate 2D tuples of fractional coordinates in the rectangle bounded by corners `(0, 0)` and `(+1, +1)`. These coordinates will be passed to a set of consumers which will *do work* and produce a single output, a fraction in the range `[0, 1]`.

## Problem and Solution Design

The number of producers (P) and the numbers of consumers (C) should be configurable. Each producer and each consumer should run in their own thread. Each generated coordinate should fall on a grid whose size/width (N) or resolution should be configurable. (e.g. x and y ∈ `{ 0, .33, .67, 1 }` for a 4×4 grid.)

As a challenge, to introduce an arbitrary source of race conditions, each point corresponding to a coordinate should have an ID number in the range `[0, N×N)` and the producers should share a single ID generator, a source of ID numbers for all N×N points. The coordinate values can be calculated using this ID.

The consumers should call a single output function with the result of their work, along with the source ID or coordinate (or both). The *work* is arbitrary and can be the calculation a simple Euclidean distance to the center (`sqrt((x - .5)^2 + (y - .5)^2)`) optionally followed by a short random-duration sleep.

As a challenge and to introduce another arbitrary source of race conditions, the work done by the output function can be made thread-unsafe, such as putting the result in a container that does not allow concurrent insertions. Some suggestions: A simple library class that disallows concurrent insertions; a manually-sorted list where each insertion entails looping through and finding the right index to insert the element (a la insertion sort); a simple container who a "progress meter" thread constantly loops through to determine the progress (or draw a graph!). In this case, it would be the progammer's responsibility to prevent these race conditions from occurring.

As a challenge, the main thread, after creating and starting the producers and the consumers, can wait *exactly* until all the work is done. As a further challenge, this could be done without inspecting the contents of any output container.
