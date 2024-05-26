17.05.2024 - Omar Richi (omar.richi@rwth-aachen.de)

### Question 1:

**a)** What are the tasks of memory management in operating systems? 

**b)** What tasks does the MMU perform? What is the purpose of dividing a virtual address into segment number (or page number) and offset?  
**c)** Is it possible that a virtual address is called for which no physical address exists? If so, how should the MMU handle it? 

**d)** Why is it beneficial to implement the MMU as a hardware component rather than performing the necessary calculations in software?

*Solution:*

**a)**
- Translating virtual addresses into physical ones  
- Allocating memory to processes, releasing memory  
- Preventing unauthorized memory accesses  
- Paging memory to or from secondary storage  
- Facilitating shared memory

**b)**
- The MMU translates logical addresses into physical ones
- It is located between the CPU and memory
- Through segmentation, the size of the mapping table of the MMU is reduced
	- There doesn't need to be an entry in the table for every possible address, but only for every possible address of a segment/page

**c)**
- This can happen and is a normal case, so it occurs regularly
- It means a process wants to access a memory location that is not loaded into the main memory
- Causes a page fault
    - Page fault: The MMU translates and searches for the requested address, and it is then loaded into the main memory

**d)**
- Speed! Accesses to virtual addresses occur very frequently and must be converted into accesses to physical addresses as quickly as possible, otherwise the entire process execution will be slowed down.