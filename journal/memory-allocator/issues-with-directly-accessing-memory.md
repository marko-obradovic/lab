When you're accessing memory manually on your OS with C, you're actually running your program in user mode - the OS will restrict what you can do. You cannot:

- Arbitrarily read or write physical RAM
- Directly control hardware devices
- Modify the memory of another process without permission
- Execute privileged CPU instructions

If you try to access an arbitrary physical address:

```
int *ptr = (int *)0x12345678;
*ptr = 42;
```

You aren't necessarily accessing physical address `0x12345678`.

You're attempting to access virtual address `0x12345678` in your process's address space.

**Kernel mode**
![[Pasted image 20261122123131.png]]

When you're doing kernel programming, you're actually accessing physical memory.

# Issues with directly accessing memory
## Security
Because all applications are sharing the same memory space, no one prevents one program from changing or accessing the memory of another program.

> **Fun fact:** First version of Windows, up to 3.11 didn't have any protection against this. Win 95, 98, and Millennium edition provided some, but they still allowed direct access to memory.

Any program, whether malicious or poorly written, could overwrite critical system memory, which could cause crashes, data corruption, or even hardware instability.

## Fragmentation
Let's say you have a system with a contiguous block of 1 GB of free RAM.

The system loads 3 programs:
- The first program requires 200 MB memory
- The second requires 300 MB memory
- The third requires 250 MB memory

Each of these are loaded and running, and you have 274 GB free RAM left to use for something else.

![[Pasted image 20261121075759.png]]

Now the first program finishes executing, and is released from memory. Now you have free 200 MB at the start, and a 274 MB at the end:

![[Pasted image 20261121080147.png]]

Now a fourth program comes along, requesting 350 MB, and gets rejected. Even though we have a total of 474 MB of memory, it can't find 350 MB of contiguous memory.

## Insufficient Memory
If lots of programs run in parallel, you can run out of memory (this is still a problem though)

# Wasting memory on loading the same code multiple times
For example, common C library used by multiple processes would need to be loaded in separately for each process, even though they all use the same code. This not only wastes memory, but causes programs to start slower, because each program has to load all common libraries into its own allocated memory space

