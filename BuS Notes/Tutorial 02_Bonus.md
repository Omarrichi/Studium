## Bonus: Segmentation vs. Paging

Given a computer with *16 GiB* main memory and *64-bit* address space. You are considering whether to use segmentation or paging. You have the choice between 3 options


Note : 1 GiB = $2^{30} bytes, 1 MiB =2^{20}bytes$

*Option 1: Paging with pages of 4 bytes in size*
**1.1** How many entries would there be in the page table?

**1.2** Why such a small page size could cause a lot of TLB misses.

**1.3** What is the size of the page table for the single translation, with 1 level of page table?

**1.4** What about 2 levels of page table?

**1.5** What about 7 levels of page table?

**1.6** Why is this number of levels problematic?

*Solution:*

**1.1** With an address space of 64 bits, there are $2^{64}$ possible virtual addresses, each containing one byte. These are then divided into pages of 4 byte in size, resulting in $\frac{2^{64}}{2^2}=2^{62}$ pages.  

**1.2** Since the page size is so small (4), the translation is shared by only 4 virtual addresses. (a single entry for 4 addresses in the page table). As the page size decreases, fewer addresses share the same translation, resulting in a greater variety of translations and consequently, an increased rate of TLB misses.

**1.3** Since we have a single level of page table, we need only 1 table of $2^{62}$ entries. 1 address is $2^3$ bytes so we would end up with $2^{65}$ bytes of memory for a single translation. This is not possible on a 64 bit machine.

**1.4** With two level page table, we need to split the 62 bits in two. We end up with two page tables of $2^{31}$ entries. The total size for a single address translation is at least one 1st-level page table and one  
2nd-level page table. We end up with a total size of $2\times 2^{31}\times 2^3 = 2^{35} = 32$ GiB  

**1.5** With seven levels, we need to split the 62 bits in $7: 62=7\times 8 + 6$.

We have 6 bits unused (this means that some addresses can’t be translated anymore, but this is not a big deal, $2^{56}$ pages or $2^{58}$ addresses is already more than enough for each process; on a modern system, processes only have access to $2^{48}$ virtual memory addresses) 

Each of the seven tables contains $2^8$ entries of $2^3$ bytes, about 2 KiB per table. 

7 tables would be 14 KiB in total.  

A single translation would require only 14 KiB with 7 level page table.

**1.6** We have way too much computational overhead from consulting all the page tables each time.

But at least, there would be virtually no problems with fragmentation: external fragmentation does not occur with paging anyway, and since memory can be requested in multiples of 4 bytes, there is not much internal fragmentation either

*Option 2: Paging with pages of 256 MiB in size*

**2.1** How many entries would there be in the page table

**2.2** Why is having pages of this size problematic?

*Solution:*

**2.1** If we split the entire address space of $2^{64}$ addresses by group of $256 Mib = 2^{28}$, we end up with $\frac{2^{64}}{2^{28}}=2^{36}$ different pages. The page table needs to hold $2^{36}$ entries.

**2.2** Again, there is no problem with external fragmentation. However, there is likely to be a lot of internal fragmentation, as you can only ever request multiples of 256 MiB.

```ad-note
title: Side note:
We should also increase the number of levels in the page table
```

