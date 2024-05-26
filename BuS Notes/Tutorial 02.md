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
	- 