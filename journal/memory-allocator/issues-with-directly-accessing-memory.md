Reference: [Virtual Memory Explained (including Paging)](https://www.youtube.com/watch?v=fGP6VHxqkIM)
# Issues with directly accessing memory
#### Security
Because all applications are sharing the same memory space, one program can change / access the memory of another program.

> **Fun fact:** First version of Windows, up to 3.11 didn't have any protection against this. Win 95, 98, and Millennium edition provided some, but they still allowed direct access to memory.

Any program, whether malicious or poorly written, could overwrite critical system memory, which could cause crashes, data corruption, or even hardware instability.

#### Fragmentation
Imagine a system with a contiguous block of 1 GB of free RAM.

The system loads 3 programs:
- The first program requires 200 MB memory
- The second requires 300 MB memory
- The third requires 250 MB memory

Each of these are loaded and running, and you have 274 GB free RAM left to use for something else.

![[Pasted image 20261121075759.png]]

Now the first program finishes executing, and is released from memory. Now you have free 200 MB at the start, and a 274 MB at the end:

![[Pasted image 20261121080147.png]]

Now a fourth program comes along, requesting 350 MB, and gets rejected. Even though we have a total of 474 MB of memory, it can't find 350 MB of contiguous memory.

#### Insufficient Memory
If lots of programs run in parallel, you can run out of memory (this is still a problem though)

#### Wasting memory on loading the same code multiple times
E.g. A common C library used by multiple processes would need to be loaded in separately for each process.

This:
- Wastes memory
- Causes programs to start slower (each program has to load all common libraries into its own allocated memory space)

#### Full programs would need to be loaded into RAM before execution begins
SInce entire programs need to fit into memory, large programs might not run when many other programs are loaded into memory.