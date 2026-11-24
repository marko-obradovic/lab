Pages in virtual memory are mapped to frames in physical memory using a special data structure that holds information for each page - a **Page Table**

Every Page Table entry includes the physical address of that address in RAM, and [FINISH THIS]

The number of entries in a single Page table can be huge. For example:

If the virtual memory space has a 
> [!question] Does this mean the virtual memory space of a single application? Do you have different amount of virtual memory space that applications get? Does it matter how big the virtual memory space is as long as it has as many pages as frames? I assume this isn't some collective virtual memory, since applications must have their own virtual memory to prevent modifying / accessing other applications' physical memory spaces.

