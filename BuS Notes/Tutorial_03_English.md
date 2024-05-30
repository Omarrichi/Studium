30.05.2024 - Omar Richi (omar.richi@rwth-aachen.de)

*Basics:* 

file management:
1. File abstraction
2. Directory abstraction
3. File systems
4. Caching

Volatile vs. Non-Volatile Memory

Need for Abstraction
- Every device work differently
- Can't expose directly to users

Main Abstraction for users $\Rightarrow$ files
OS component to manage files $\Rightarrow$ file System

---
File:
- name
- structure
- types
- attributes
- operations
- descriptors

File Types: 
- Regular files
- Directories

File Operations: 
- Create
- Delete
- Get/Set attributes
- Rename
- Open: Returns a *file descriptor*
- Close
- Read/Write
- Seek

File descriptor:
- state of currently open file
- multi open == multi file descriptors

![[Pasted image 20240530203736.png]]

---

Paths:
- Absolute: starts from the root of the system up to the file
- relativ: starts from the current location, the current working directory (cwd) 

---

Hard Links and Symbolic Links:
- Hard Link: Copy the file $\Rightarrow$ a file can appear in multiple directories
- Symbolic Link: special file containing a path to another file
	- when Symbolic Link is deleted it does not delete the file is it pointing at

---

### Introduction:

*Question 1:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 1:*

*Question 2:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 2:*

*Question 3:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 3:*

*Question 4:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 4:*

*Question 5:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 5:*

*Question 6:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 6:*

*Question 7:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 7:*

*Question 8:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 8:*

*Question 9:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 9:*

*Question 10:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

*Solution 10:*