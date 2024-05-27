17.05.2024 - Omar Richi (omar.richi@rwth-aachen.de)

### 1.1 The MMU:

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

### 1.2 System Preformance:

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

First of all, why is the CPU not being efficiently loaded? It is not because of I/O-heavy processes, as 5% is   hardly any access to I/O devices. But the access to the background memory (97,7%) is as high as it gets, therefore apparently the CPU cannot compute more because pages have to be constantly reloaded for the processes. This means we have to reduce the page fault rate.

1. This doesn't help; our CPU is already not fully utilized. A faster CPU would be even less utilized, and the speed of loading pages doesn't change here.
2. This also doesn't help. Memory space is not the problem; the problem is the speed.
    - You can imagine: if our pages are like items in a warehouse and the door is the size of a normal door, making the warehouse bigger doesn't automatically mean we have a bigger door (or shoes in running mode :) ).
3. More processes mean more page requests, which means more page faults, so this doesn't help either.
4. Similar to three but in reverse: fewer processes mean fewer requests, so fewer page faults. This will help.
5. More main memory means more space for our pages, so we will need to swap pages less often $\Rightarrow$ fewer page faults, so this will also help.
6. This will help; although we still have the same number of page faults, the CPU can get the data faster.
    - Similar to 2: the warehouse remains the same size, but the door gets bigger, allowing faster movement in and out (running mode is also a good analogy here).

###  2. Segmentation:

For memory management using segmentation, the following segment table is given for a process

|  segment  | base address | length |
|:---------:|:------------:|:------:|
| ```0x0``` | ```0x528```  |  320   |
| ```0x1``` | ```0x232```  |   58   |
| ```0x2``` | ```0x136```  |  120   |
| ```0x3``` | ```0x2ee```  |   60   |
| ```0x4``` | ```0x33e```  |  150   |
| ```0x5``` | ```0x3de```  |  110   |


Both the base address and the length are given in bytes.  

**a)** How many bytes are available to the process in physical memory?  

**b)** The address is coded on 16 bits. The maximum segment number is 16, how would you split the virtual address between the segment number and the address within the segment? What is the maximum segment length?

**c)** The process requests the address ```0x31``` within segment number 2, what virtual address will be sent to the MMU?

**d)** At what time is the virtual address from question *c)* determined?

**e)** If two separate processes each have a memory segment labeled with the same segment number within their address space, how does the operating system ensure that these segments do not overlap in physical memory?

**f)** The following virtual addresses are now requested by the process. What does the MMU return in each case?
- ```0x301e```
- ```0x1012```
- ```0x207e```
- ```0x4095```

**a)** Simply taking the sum of the last column results in 818 bytes. (320 + 58 + 120 + 60 + 150 + 110 = 818)

**b)** The segment numbers go up to 0xf (16 segments so 15 + 1), so we need at least 4 bits to store this value. With 4 bits, we can store segment numbers from 0x0 to 0xf. We use the remaining 12 bits for the address within the segment. The address is stored in 12 bits, ranging between 0x000 and 0xfff. Therefore, the maximum segment length is 0xfff + 1 = 4096.

**c)** We take the segment number 0x2 and use the last 12 bits for the address: 0x2031.

**d)** This is done transparently during compilation.

**e)** In operating systems with segmentation, each process is linked to a unique segment table, which ensures memory isolation by assigning distinct base addresses to each segment. During a context switch, the operating system loads the segment table of the selected process into the Memory Management Unit (MMU). This setup guarantees that even if different processes use the same segment numbers, they will not access the same physical memory locations, since each process operates with its own segment table.

**f)**

- ```0x301e```: The segment is ```0x3``` and the address is ```0x01e```. We take the base address for segment 3: ```0x2ee```, and sum it with the address: the MMU returns ```0x30c```. 
- ```0x1012```: The segment number is 1, ```0x012``` + ```0x232``` = ```0x244```. ```0x207a```: The segment number is 2, and the address is ```0x07a``` (or 122). But the length of segment 2 is 120, and 122 exceeds the segment length. This leads to an invalid memory access: the MMU sends a segmentation fault interrupt to the CPU. 
- ```0x4095```: The segment number is 4, ```0x33e``` + ```0x095``` = ```0x3d3```.

### 3. Paging:

```ad-note
title: a reminder for the bitwise operations in C
```

![[Pasted image 20240526171811.png]]

Suppose we have a 32 bits System, managing virtual memory with paging. The page size is 4KiB and the TLB is empty.

The system uses a two level page table, the virtual addresses are divided as follows:
- 10 bits for the index of the first level page table  
- 10 bits for the index of the second level page table  
- 12 bits for the offset within the page

For instance, the address ```0xc01b36e4``` is divided like this: ```0b1100000000|0110110011|011011100100```

- the index for the first level of the page table is 768  
- the index for the second level is 435  
- the offset is 1764
The page Table Entry is composed of 20 bits for the page frame number, and the last 12 bits are used for metadata

PTE

![[Pasted image 20240526172147.png]]

The last three bits represent the following flags:

- U/S: set to 1 if the page can be accessed either in user or supervisor mode  
- R/W: set to 1 if the page is r/w, 0 if read-only  
- P: set to 1 if the page is present in memory

![[Pasted image 20240526172231.png]]

Suppose that a task is running, the page table pointer of the task contains 
```0xef2e5000```


*Questions:*
1. The third page displayed above starts at the address ```0x1432c000 ```. What is the associated page number?
2. The value ```0x6cb4f481``` is stored inside the first page, at the index 4. What is the virtual address at which the value is stored?
3. What is the size of each page table level? Why is it convenient?
4. What is the total size of memory used by the page table for a single address translation?

*Solution:*

1. Each page is 4KiB in size, or ```0x1000``` bytes. We can determine the page number; ```0x1432c000``` // ```0x1000``` = ```0x1432x```
2. The value is located in the page number ```0xef2e5``` at the index 4. Each entry is 4 bytes in size. (because we have a 32 bit system, each the page table needs to store addresses of 32 bits). The virtual address is 0xef2e5000 + ($4*4$) = ```0xef2e5000``` + 16 = ```0xef2e5000``` + ```0x10``` = ```0xef2e5010```
3. A single page level table is $2^{10}$ entries of $2^2$ bytes (Because we are on a 32 bits system). The size of a page table is $2^2 * 2^{10} = 4 KiB$. This is convenient because this is the size of a page.
4. A single translation corresponds to 1 first level page table (4 KiB) and 1 second level page table (4 KiB). Total size of 8 KiB.

*Tasks:* 
The CPU is performing the following actions:

1. In user mode, read at the address ```
0x97ea0329```
2. In user mode, write to the address ```0x5fbc6582```
3. In kernel mode, read at the address``` 0x5fbcd6f0```
4. In user mode, read at the address ```0x5fbd2bad```
5. In user mode, read at the address ```0x97ffbc21```
6. In user mode, read at the address ```0x97ea0fe2```

For each action, document the steps involved in MMU translation. Explain how the MMU accesses the Page Table Entry associated with the virtual address. translates the virtual address to a physical address, and handles any interrupts or exceptions encountered during the translation process.

TLB (Translation Lookaside Buffe)