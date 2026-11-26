# Summary

> **Physical memory:** The literal RAM hardware

> **Virtual memory:** A mapping system that lets each process think it has its own large, private memory space, while the OS dynamically decides which parts live in RAM (fast), and which parts live on the disk (slow)

We DON'T want to access physical memory generally, so our OS protects us from accessing it. Instead, it implements virtual memory.

In order to stop external fragmentation from happening, we use paging, where we break up the:
- Virtual memory into 4 KB pages
- Physical memory into 4 KB frames

For every application, the app will be split up and distributed into these 4 KB pages in its own virtual memory space.

Every single one of those pages are mapped in the background (using a page table) to a physical location in the RAM (one of the frames).

For every application, there isn't a one-size virtual memory space - it grows and shrinks based on how much memory the application actually needs.

If an application needs more pages than there is available RAM, the OS moves inactive pages from RAM to swap on disk. If you try to access those pages again, you get a page fault, and the OS loads them back into the RAM (evicting something else if needed), and execution resumes. But the application can technically have a larger amount of pages than available frames in physical RAM, because it might be mapping some pages to frame in hard drive storage.

# Notes
The page about [[issues-with-directly-accessing-memory|issues with directly accessing memory]] outlines what happens when we don't have virtual memory, and the page on the [[solution-to-physical-memory-access|solution to physical memory access]] outlines how we tackled those issues.

The page on [[how-virtual-memory-works|how virtual memory works]] goes into more details about **Virtual Memory**

## Extra
When you're accessing memory manually on your OS with C, you're using virtual memory. You're actually running your program in user mode - the OS will restrict what you can do. You cannot:

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