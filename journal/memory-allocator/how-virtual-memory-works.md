Reference: [Page Tables and MMU: How Virtual Memory Actually Works Behind the Scenes (Animation)](https://www.youtube.com/watch?v=B6tJxvYBNrU)

Pages in virtual memory are mapped to frames in physical memory using a special data structure that holds information for each page - a **Page Table**

Every Page Table entry includes the physical address of that address in RAM, and some additional data outlined in [[page-content]]

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

**Remember:**
- **Page table:** The actual data structure containing the virtual-page-to-physical-frame mappings.
- **Base address:** The physical RAM address where that data structure begins.
- **PTBR:** A CPU register that holds that base address, not the page table itself.

You can only have one page table active at a time.

In this model, changing the active page table is as simple as updating this register to reference the current process' page table.

Problem is, if the processor has to access RAM just to translate the virtual address to the physical address, that means for every memory read/write request, two memory accesses are needed. So in this model:
1. The application gains access of the CPU (is executing on the CPU)
2. The PTBR points to the base address of the top-level page table (particularly important when we get to multi-level page tables)
3. The CPU would use that base address to accesses the page table in RAM to find the physical frame corresponding to the virtual page.
4. The CPU combines the physical frame address with the page offset
> [!question] what does this mean
5. The CPU accesses the physical frame in the RAM again to retrieve or modify the actual data.