# The Solution to Physical Memory Access
Operating Systems implement **memory access indirection**

Instead of accessing a memory address based on its physical location in memory, the OS creates a virtual address space, and maps it to physical memory.

In a multi-process system, each process has its own virtual memory space, and is unaware of how the OS maps its virtual memory to physical memory.

Since the process can't access physical memory, it can't modify data belonging to other processes, or interfere with their execution.

Taking control of physical memory access to individual processes, and giving it to the OS significantly increases the security and stability of the entire system.

#### Solving the insufficient memory problem
Running multiple memory-intensive applications quickly consumes memory. With Virtual Memory, the OS can extend available memory by using disk space.

A special file called the **swap file** (or page file) serves as an extension of main memory.

A program always reads / writes from its virtual memory space, but the actual data could be mapped to either physical RAM, or to a Hard Disk.

If enough physical memory is available, a program is loaded and executed normally.

If memory is scarce, the OS swaps some data to disk, making room for new processes.

This mechanism is called swapping - it allows the system to run apps that exceed their physical RAM capacity at the cost of performance.

## Paging
Mapping every virtual byte to a physical byte would require enormous address translation tables.

Instead, virtual memory systems use paging as a way to reduce the size of these translation tables.

> **Paging:** A memory management scheme where
> 	- **Virtual memory** is divided into fixed sized blocks called pages (typically 4kB)
> 	- **Physical memory** is divided into blocks of the same size called frames.

When a program is loaded, its pages are loaded into any frames in physical memory, regardless of whether they're contiguous or not.

This approach eliminates the external fragmentation issue, but creates a problem called internal fragmentation.

> **Internal Fragmentation:** Where available space at the end of a page is unused, because the memory required by the program is not perfectly aligned with the page size.

With paging, common libraries no longer need to be loaded separately for each process. All common libraries can be loaded into a set of frames in physical memory and be mapped to by multiple different processes which allows them to share common pages called **Shared Pages**

#### Issue with Internal Fragmentation
**You waste physical RAM that could otherwise be used by something else.** There's no inherent problem for the program. It doesn't crash, reject the allocation, or require every byte of a page to be occupied.

For example, imagine you have **16 GB of RAM**, and Your OS uses **4 KB pages**.

Suppose you have 1,000 processes, each with a memory allocation that wastes an average of 2 KB in its final page.

That's: $1000 \times 2\text{ KB} = 2\text{ MB}$ - You've lost 2 MB of RAM to internal fragmentation.

Now imagine this at much larger scales, with millions of allocations and many different memory regions. The wasted memory adds up.

With memory divided into small chunks, it's possible to load only the critical parts of a program before execution, which minimizes the amount of memory used at any given moment. More pages can be loaded as needed during program execution. This is called **Demand Paging**