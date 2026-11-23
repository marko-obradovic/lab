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

## Wasting memory on loading the same code multiple times
For example, common C library used by multiple processes would need to be loaded in separately for each process, even though they all use the same code. This not only wastes memory, but causes programs to start slower, because each program has to load all common libraries into its own allocated memory space

## Entire programs would need to be loaded into RAM before execution begins
SInce entire programs need to be fit into memory, large programs might not run when many other programs are loaded into memory.


# The Solution
To solve all these problems, Operating Systems implement **memory access indirection** - instead of access a memory address based on its physical location in memory, the OS creates a virtual address space, and maps it to physical memory.

In a multi-process system, each process has its own virtual memory space, and is unaware of how the OS maps its virtual memory to physical memory.

Since the process can't access physical memory, it can't modify data belonging to other processes, or interfere with their execution.

Taking control of physical memory access to individual processes, and giving it to the OS significantly increases the security and stability of the entire system.

## Solving the insufficient memory problem
Running multiple memory-intensive applications quickly consumes memory. With Virtual Memory, the OS can extend available memory by using disk space.

A special file called the **swap file** (or page file) serves as an extension of main memory.

A program always reads / writes from its virtual memory space, but the actual data could be mapped to either physical RAM, or to a Hard Disk.

If enough physical memory is available, a program is loaded and executed normally.

If memory is scarce, the OS swaps some data to disk, making room for new processes.

This mechanism is called swapping - it allows the system to run apps that exceed their physical RAM capacity at the cost of performance.

## Paging
Mapping every virtual byte to a physical byte would require enormous address translation tables, so instead, virtual memory systems use paging as a way to reduce the size of these translation tables.

Paging is a memory management scheme in which virtual memory is divided into fixed sized blocks called pages, whereas physical memory is divided into blocks of the same size called frames.

Most modern OS use 4kB pages and frames, but smaller / larger sizes can be used

When a program is loaded, its pages are loaded into any frames in physical memory, regardless of whether they're contiguous or not.

Although this approach eliminates the external fragmentation issue, it also creates a problem called internal fragmentation, where available space at the end of a page is unused, because the memory required by the program is not perfectly aligned with the page size.
> [!question] I get this, but what problem comes up here? As far as I understand (could be wrong) memory is allocated to pages linearly, and will just build on top of the next (why can't physical memory have the same thing? Can't things just be shifted to the left and onstantly make space for contiguous memory?), but that would mean there's only one 4kB page that would have this issue in virtual memory? And what's wrong with that? Will the program bug out, because it literally NEEDS 4kB of memory to fill up that page, otherwise it's like, no too little? Will it bug out because, actually it can, but when it maps it to physical frames, the FRAMES need exactly 4kB otherwise it rejects it?

With memory divided into small chunks, it's possible to load only the critical parts of a program before execution, <!--c:05p9g-->which minimizes the amount of memory used at any given moment.<!--/c:05p9g--><!--co:05p9g by:me at:2026-11-23T07:07:29.226Z status:open quote:"which minimizes the amount of memory used at any given moment"
  me (2026-11-23T07:07:29.226Z): How?
--> More pages can be loaded as needed during program execution. This is called **Demand Paging**

Another advantage of paging....

> [!question] 
> - How does a program assign data to pages in virtual memory
> - How does virtual memory assigns pages to frames in physical memory
>
> Apparently this book is excellent, and free: [Operating Systems: Three Easy Pieces](https://pages.cs.wisc.edu/~remzi/OSTEP/)

