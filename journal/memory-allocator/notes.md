First I need to figure out what I need to do for this project.

**Questions:**
1. What's the most basic small application I can use my memory management library for?
	1. Simple text editor - vim clone
	2. Hash table - basically just a dictionary
2. How do I create Unit tests for this? What tech do we use for unit tests?
3. When finished, how can I:
	1. Make the project look professional and clean
	2. Make sure everything is efficient, and to standards (do AI code reviews)
# Memory Theory

## Memory and CPU Interactions
The CPU needs instructions to execute. It gets these instructions from the RAM - from what I understand it reaches out into the <!--c:a9kxa-->instructions cache<!--/c:a9kxa--><!--co:a9kxa by:me at:2026-11-20T21:17:41.778Z status:open quote:"instructions cache"
me (2026-11-20T21:17:41.778Z): Where is this? is it a component of the RAM? Or does this mean that L1, L2 or L3 cache in the CPU is dedicated towards instructions cache storage? Or something else?

-->

These instructions are gonna be in the form of binary - it's gonna use a <!--c:ahhk1-->decoder<!--/c:ahhk1-->
<!--co:ahhk1 by:me at:2026-11-21T06:27:01.536Z status:open quote:"decoder"
me (2026-11-21T06:27:01.536Z): is h is built in?
-->




# Problems with manual memory allocation
Reference: [Virtual Memory Explained (including Paging)](https://www.youtube.com/watch?v=fGP6VHxqkIM)

Let's say you have a computer where all memory access is done using the actual physical address in memory (in the RAM)

Direct memory access creates several problems, outlined in [[manual-memory-management-issues]]


# Creating a Library in C
``

# Components of Memory Management
Python actually uses three components for memory management. We have:
1. Memory Allocator
	- Where do I put things?
2. Memory Manager
	- What things do I have, and where are they?
3. Garbage Collector
	- Which things can I throw away because no one needs them anymore?


### Warehouse analogy:
**1. Memory allocator — finding space**
Imagine you've got a warehouse full of shelves.

Someone comes in and says:
"I need somewhere to store a box that is 20cm wide."

Your job is to:
- Find an available space.
- Reserve it.
- Tell them where it is.
- Make that space available again when they're finished.

That's a memory allocator.
In C, `malloc()` and `free()` provide this functionality.

**The problem it solves:** How do I efficiently reserve and release chunks of memory?

**2. Memory manager — keeping track of everything**
Now imagine you're running that warehouse.

You don't just need to find shelf space. You need to know what's actually in the warehouse.

For example:

- Shelf A contains someone's furniture.
- Shelf B contains electronics.
- Shelf C contains food.
- Shelf D is empty.

You maintain an inventory.

A memory manager does something similar. It keeps track of objects and their associated memory.

For example:

```
Object A
  Type: Integer
  Address: 0x1000
  Size: 4 bytes

Object B
  Type: String
  Address: 0x2000
  Size: 20 bytes

Object C
  Type: Array
  Address: 0x3000
  Size: 64 bytes
```

**The problem it solves:** How do I organise, track and manage the objects my program has created?

Importantly, a memory manager is a broader architectural concept. It might use an allocator internally rather than being a completely separate system.

**3. Garbage collector — deciding what can go**
Now imagine your warehouse is getting full.
Some boxes belong to customers who still need them.
Others belong to customers who have cancelled their orders and no longer need them.
You need to identify which boxes are no longer required and clear them out.

That's a garbage collector.

It examines which objects your program can still access and identifies those that are no longer reachable.

**The problem it solves:** How do I identify and reclaim memory that is no longer needed without requiring the programmer to manually release every allocation?