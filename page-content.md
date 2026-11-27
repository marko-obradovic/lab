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

