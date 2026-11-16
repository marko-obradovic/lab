# Project 1A - Memory Allocator
01 — Introduction
For this project, I aim to build a Memory Allocator.
This should be a small library that manages memory allocation itself, rather than relying entirely on `malloc` and `free`.

The goal is to implement:
- A fixed-size memory pool.
- A function to allocate memory.
- A function to release memory.
- Tracking of free and occupied blocks.
- Basic protection against invalid allocations.

Start with a simple bump allocator, then implement a free-list allocator.

**Technologies**
- C
- GCC or Clang
- GDB
- Make or CMAKE
- Valgrind

**What I aim to learn**
- Stack versus heap.
- Pointers and pointer arithmetic.
- Memory alignment.
- Memory fragmentation.
- Allocation strategies.
- How memory is represented and managed.

**Why this project?**
Memory management is one of those things that you can use for years without understanding particularly well.

This forces you to confront it directly.

You'll also get a taste of the sort of problems that C and C++ programmers regularly encounter.

02 — Background and Research
- How does dynamic memory allocation work?
- What is the heap?
- What do malloc and free actually do?
- How does fragmentation happen?

03 — Design
- Requirements
- Proposed architecture
- Data structures
- Design decisions

04 — Development Log
Chronological entries documenting implementation, discoveries, mistakes and problems.

05 — Testing and Evaluation
- Unit tests
- Edge cases
- Performance measurements
- Comparison against existing implementations

06 — Final Results
What works?
What doesn't?
What did I achieve?

07 — Retrospective
What did I learn?
What would I change?
What should I investigate next?
