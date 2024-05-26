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

### Question 1.2:

Given a demand paging system that is currently occupied as follows:
- CPU load: 20%
- ratio of page fault for accessing memory: 97,7%
- load of other I/O devices: 5%

The aim is to optimize the CPU load in order to process all programs fast and efficiently. for each of the following suggestions, evaluate whether it would improve the CPU load (i.e. increase):

1. install a faster CPU  
2. install a larger hard disk  
3. increase the number of running/ready processes  
4. reduce the number of running/ready processes  
5. install more main memory  
6. install a faster hard disk

*Basics:*

When a process is executed, its data is loaded by bringing the pages into the main memory. However, the main memory does not have enough space for all the pages of all the running processes, so only the necessary pages are loaded into the main memory. When a process is executed, its pages need to be loaded into the CPU, and this is done via the MMU.


- When accessing a page:
    - The MMU checks if the page already exists in the main memory.
    - If yes, it is translated (logical $\Rightarrow$ physical).
    - If no:
        - 1. The page actually exists but needs to be loaded from the hard disk into the main memory (page fault).
        - 2. The page does not exist or does not belong to this process, then access is denied (segmentation fault).
- When a page fault occurs:
    - Execute the page fault handler.
    - Put the process to sleep if necessary.
    - Locate and load the missing page (potentially evicting other pages).
- Page faults take a considerable amount of time, so avoid them!

```ad-note
title: Demand Paging
Describes the system for managing memory so that memory is only accessed when needed. In our system, we only fetch the pages we need and evict the ones we no longer need. The system cannot be optimally implemented because we cannot predict when a particular page will be requested. Therefore, we generally deal with strategies to achieve an approximation.
```

*Solution:*

First of all, why is the CPU not being efficiently loaded? It is not because of I/O-heavy processes, as 5% is   hardly any access to I/O devices. But the access to the background memory (97,7%) is as high as it gets,  
therefore apparently the CPU cannot compute more because pages have to be constantly reloaded for  
the processes. This means we have to reduce the page fault rate.