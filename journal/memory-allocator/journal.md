# Project 1A - Memory Manager
### 01 — Introduction
For this project, I aim to build an automatic Memory management tool in C, for C.

This should be a small library that manages:
- Memory allocation
- Memory management
- Garbage collection

**Technologies**
- C
- GCC or Clang
- GDB
- Make or CMAKE
- Valgrind

**Why this project?**
Memory management is one of those things that you can use for years without understanding particularly well. This forces you to confront it directly.

### 02 — Background and Research
> **Note:** Fill this in from the Development log, and other notes when the project is finished

### 03 — Design
- Requirements
- Proposed architecture
- Data structures
- Design decisions

### 04 — Development Log
> Chronological entries documenting implementation, discoveries, mistakes and problems.

**2026-11-17**
Going to zoom into memory allocation first.

Questions:
1. How does it work manually? What are core concepts I need to understand?
2. Then I'll try to focus on just understanding what automation looks like for this. What functions can I break memory allocation down to, an how can I apply it?

Currently, I'm just looking at how memory actually works. Might be going a bit too deep with it, but I'm trying to get a general sense of how RAM and the CPU work together. I'm doing this so that I have a basic idea of what's happening in the background when I'm allocating and de-allocating memory - it might make it easier to learn the features of memory allocation in C as well.

**2026-11-21**
I've been trying to understand how the CPU and Memory interact, but kept zooming into the CPU, I think it's more important to start at memory, and how RAM works - GPT suggested to look into **virtual memory, physical memory, memory addresses and page tables**. That seems to be clearing up a lot - I'm going to be looking into these things, but I'm still lost as to what the RAM's role is outside of just "you load programs into it, and your computer uses them". I really want to work towards a bigger picture here - I don't need to understand the full contents of how all components work and the physics behind them (yet), but I do need a mental model that can serve as a reference point I can constantly refer to when looking through a problem. The same way that, once I understood signal flow on an SSL desk at ICMP, I could fix any problems I was having with the hardware - I didn't know how the components worked, but I knew where things flowed to at a high level, and what to expect as inputs / outputs at least.

**2026-11-24**
I understand the high level overview of the problems that we've had with physically allocating memory, and some solutions we put in place with Virtual Memory. Now I'm going to see how Virtual Memory implements those solutions to get a better understanding of what this mapping looks like.

**2026-11-26**
Just needed to refine a few things on Virtual Memory - check git history to see how that's evolved, but I'm confident in explaining what virtual memory is, why it's used, and how pages work (high level) - I was really confused about whether each application had a fixed amount of pages, or if it grows / shrinks dynamically, and whether it was based on RAM size or not - but it's dynamic. It's not based on RAM size - the amount of pages will grow, and if the app gets an extra page that can't map to any more RAM, it'll just swap some other page out that hasn't been used to use hard drive or SSD storage, and the new page can start to use RAM.


### 05 — Testing and Evaluation
- Unit tests
- Edge cases
- Performance measurements
- Comparison against existing implementations

### 06 — Final Results
What works?
What doesn't?
What did I achieve?

### 07 — Retrospective
What did I learn?
What would I change?
What should I investigate next?
