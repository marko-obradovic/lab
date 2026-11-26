Reference: [Page Tables and MMU: How Virtual Memory Actually Works Behind the Scenes (Animation)](https://www.youtube.com/watch?v=B6tJxvYBNrU)

Pages in virtual memory are mapped to frames in physical memory using a special data structure that holds information for each page - a **Page Table**

Every Page Table entry includes the physical address of that address in RAM, and [FINISH THIS] some additional data

The number of entries in a single Page table can be huge. For example, per application, the OS might allocate:
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

