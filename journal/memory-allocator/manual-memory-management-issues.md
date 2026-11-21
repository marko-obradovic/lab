
**Security:**
Because all applications <!--c:rs5kd-->are sharing the same memory space<!--/c:rs5kd-->
<!--co:rs5kd by:me at:2026-11-21T07:49:56.290Z status:open quote:"are sharing the same memory space"
me (2026-11-21T07:49:56.290Z): As in, they share the same memory space in this example, or in general, they share the same memory space, but in this example, there's no logical separation, and in real life, there's a wall between them so that none of them can access each other?
-->, unless implemented, no one prevents one program from changing or accessing the memory of another program.

> **Fun fact:** First version of Windows, up to 3.11 didn't have any protection against this. Win 95, 98, and Millennium edition provided some, but they still allowed direct access to memory.

Any program, whether malicious or poorly written, could overwrite critical system memory, which could cause crashes, data corruption, or even hardware instability.

**Fragmentation:**
Let's say you have a system with a contiguous block of 1 GB of free RAM.

The system loads 3 programs:
- The first program requires 200 MB memory
- The second requires 300 MB memory
- The third requires 250 MB memory

Each of these are loaded and running, and you have 274 GB free RAM left to use for something else.

![[Pasted image 20261121075759.png]]

Now the first program finishes executing, and is released from memory. Now you have free 200 MB at the start, and a 274 MB at the end:

![[Pasted image 20261121080147.png]]

Now another program comes in, and it needs 350 MB.....