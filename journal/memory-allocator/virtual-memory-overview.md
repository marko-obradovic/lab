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

The page about [[issues-with-directly-accessing-memory|issues with directly accessing memory]] outlines what happens when we don't have virtual memory, and the page on the [[solution-to-physical-memory-access|solution to physical memory access]] outlines how we tackled those issues.

The page on [[how-virtual-memory-works|how virtual memory works]] goes into more details about **Virtual Memory**