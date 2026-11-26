Reference: [Page Tables and MMU: How Virtual Memory Actually Works Behind the Scenes (Animation)](https://www.youtube.com/watch?v=B6tJxvYBNrU)

Pages in virtual memory are mapped to frames in physical memory using a special data structure that holds information for each page - a **Page Table**

Every Page Table entry includes the physical address of that address in RAM, and [FINISH THIS] some additional data

The number of entries in a single Page table can be huge. For example, per application, the OS could allocate:
- Virtual memory space up a capacity of 16 GB
- Page size = 4 KB
$$
\frac{16 \text{ GB} }{4 \text{ KB}} = \frac{16 \times 2^{30} \text{ B}}{4 \times 2^{10} \text{ B}} = 4 \times 2^{20} = 2^{22} = 4,194,304 \text{ Pages}
$$

| Page number | Frame number | Protection | Status |
| ----------- | ------------ | ---------- | ------ |
| 0x004B0200  |              |            |        |
| 0x004B0204  |              |            |        |

Operating Systems keep page tables stored in physical RAM within Kernel space - keeps them out of user space so user processes cannot directly tamper with address translations.

There's a problem though.

Every application has its own virtual memory space. In a system that can store page tables storing $2^{22}$ pages, with 4 KB per page, you'll get:

$$
2^{22} \text{ pages} \times 4 \text{ KB} = 16,777,216 \text{ KB}
$$

Which is $16 \text { MB}$ per table.

Multiply that by 50 processes running, and you get:

$$
16 \text{ MB} \times 50 \text{ processes} = 800 \text{ MB}
$$
This is a lot of memory space taken up just for mappings from virtual pages to physical frames.

There is no solution for this, only some clever optimizations that systems use to work around it.

> [!note] Add notes on this

For simplicity, let's assume:
- Each process has a full page table representing its virtual memory space
- Each page table is stored as an array at a fixed location in main memory (not always true for modern systems)

Whenever a process is given control of the processor (called a context switch), the active page table must be updated via a **pointer** called the **page table base register**, which is one of the CPU's protected registers.
> [!question] As in, you can only have one page table active at a time, and which page table is being used must be updated?

In this model, changing the active page table is as simple as updating this register to reference the current process' page table.
> [!question] How does it actually work in real life?

Each entry in the page table includes several fields:
- Physical frame number - the result of translating a virtual page number
> [!question] As in it will show what physical frame number this virtual page is mapped to?
- Protection (permission bits)
	- Read-only Bit (determines whether this page can be modified or not)
		- Protects pages from being overwritten - either by accident or maliciously
	- User / Supervisor Bit (determines the privilege level required to access the page)
		- The page marked as user mode is accessible to user level or privileged code, but only if it's mapped in the process' page table. 
		- A page marked as supervisor mode can only be accessed by privileged code, which is typically the OS. This helps prevent user level processes (normal every-day apps) from interfering with, or reading sensitive OS data.
	- No Execute Bit (controls whether code from this page can be executed or not)
		- This is a security feature used to mitigate certain types of attacks.
- Status bits (mainly used by dedicated hardware called the memory management unit (MMU))
	- Present / Valid bit (indicates whether the page table entry contains a valid physical frame number)
		- if the entry is invalid, it can't be used for translation
	- Dirty / modified bit (indicates whether the page has been modified since it was loaded into memory)
		- Important for knowing if a page needs to be written from main memory back to disk in the future
	- Accessed bit (indicates whether the page has been read or written to recently)
		- Helps the MMU track page usage for future caching and page replacement decisions at the hardware level
	- The global bit (set by the OS - indicates that the page is shared across all processes)
		- This flag is often used for the OS' Kernel pages.
> [!question] What does this look like in practice? How is it implemented at a high level? And how are all three of these sub-points broken down into this field, how is it parsed? What does it look like? Can it only be one bit at a time, or can you have multiple bits and it figures out what properties they have after?