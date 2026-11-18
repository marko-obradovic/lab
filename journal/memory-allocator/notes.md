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


What problem is memory allocation trying to solve?


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