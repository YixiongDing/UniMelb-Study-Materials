# Modelling Complex Software Systems Notes

Summary is wriiten by Yixiong Ding  
The University of Melbourne  
June, 2019   
_ _ _

## 1. Concurrency

### Concurrent Program
1. Sequential program:
    - has a single thread of control, that is, a single instruction pointer suffices to manage its execution
2. Concurrent program:
    - allows multiple threads of control.
    - each thread, or process, in a concurrent program often either shares data or communicates with one or more other threads in that program

### Parallelism vs concurrency
1. A program written in a concurrent programming language may be executed with or without actual parallelism; on a single processor typically by time-sharing.
2. Conversely, a program written in a sequential programming language may be executed with parallelism; for example, via vectorization

### Why Concurrency
1. A natural model
2. Necessity
3. Performance

### Concurrent programming is hard
1. **Communication**: Processes generally need to communicate with each other, either by accessing shared data, or by message passing
2. **Synchronization**: Processes may need to synchronize certain events, such as “P must not reach point p until after Q has reached point q.”

### Non-determinism
1. A major source of frustration is the fact that the execution of a concurrent program is non-deterministic
2. Unlike sequential programs, a concurrent program may be **speed-dependent** — its behaviour may depend on the relative speeds of its components’ execution
3. Small, random fluctuations in processor or input-output speed are sources of observed non-determinism

### Concurrent programming language paradigms
1. Shared-memory: assignment 1a
    - Utilising the concept of monitors.
    - These have a long history from Concurrent Pascal to Java and C#
2. Message-passing: assignment 1b
    - Based on Hoare’s idea of Communication Sequential Processes (CSP).
    - Examples include Occam, Erlang and Go

### Atomicity
In most programming languages an assignment such as n := n+1 is not atomic; a compiler will break it up into more basic instructions.

### Interference
Interleaving causes problems because there are many different
possible interleavings that can occur, and each execution may yield a different interleaving. However: 
*A concurrent program must be correct for all possible interleavings.*

### Safety and liveness properties
Properties of concurrent systems:
1. **Safety**: “nothing bad will ever happen”
    - **Interference** (or rather its absence) is an archetypal safety property
2. **Liveness**: “something good eventually happens”
    - **Deadlock** (or rather its absence) is an archetypal liveness property

### Summary
1. Concurrency is **potential** parallelism
2. Concurrency is an **abstraction** that makes it easier to reason about the dynamic behaviour of a system 
3. Formal approaches to understanding concurrent systems are required, because their behaviour is often **non-deterministic**.
4. In a concurrent program, atomic operations can be interleaved arbitrarily.
5. For a concurrent program to be correct, it must be correct for **all** possible interleavings.