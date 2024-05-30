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

Basic file system (like FAT and EXT seen during the lecture) copy the file block by block for the syscall `copy`, and they create a new inode associated with the new copied file. On the other hand, renaming a file is just updating the name of the file inside the directory, the only IO happens when getting the directory block to update the file name. This is much more efficient, especially when dealing with big files.

*Question 2:*

If `/home/root/` is the current working directory, what is the absolute path name for the file whose relative path name is `../redha/BuS/sched.txt`

*Solution 2:*

The full path name is `/home/redha/BuS/sched.txt` because the special path character `..` goes one directory behind, relative to `/home/root/`

*Question 3:*

Contiguous allocation of files leads to disk fragmentation, because some space in the last disk block will be wasted in files whose length is not an integral number of blocks. Describe the internal and external fragmentation during the allocation and deletion of block.s

*Solution 3:*

We have internal fragmentation because there is wasted space inside allocated blocks, if the file is not exactly a multiple of the block size. The moment we start to delete files, we will be deleting blocks and create external fragmentation as well.

*Question 4:*

A UNIX file system has 4 KiB blocks and 4-bytes disk addresses. What is the maximum file size in bytes if the inode contains 10 direct entries, and one single, double and triple indirect entry each?

![[Pasted image 20240530204952.png]]

*Solution 4:*

Parameters:

- **Block size**: $4 KiB$ (4096 bytes)
- **Disk address size**: $4 bytes$ (32-bit addresses)
- **Inode structure**:
    - 10 direct entries
    - 1 single indirect entry
    - 1 double indirect entry
    - 1 triple indirect entry

Direct Entries:

Each direct entry points directly to a 4 KiB block.

- Number of direct entries = 10
- Total storage from direct entries = $10 \times 4096$ bytes $10 \times 4096$  bytes  $10 \times 4096$ bytes.

A single indirect entry points to a block that contains disk addresses of other data blocks.

- Number of addresses in a single block = $\frac{4096 \text{bytes}}{4 \text{ bytes/address}}=1024$

- Each of these 1024 addresses points to a 4 KiB block.

- Total storage from a single indirect entry = - $1024 \times 4096$ bytes.

$1024^2 * 4$ KiB for the double indirect block
$1024^3 * 4$ KiB for the triple indirect block

For a total of 4402345713664 bytes (around 4 TB)

*Question 5:*

Can you spot the hard links in these directory entries? What would the file `/bin/sh` contain if it was a symbolic link to `bash`?

*Solution 5:*

`/bin/sh` and `/usr/bin/sh` contain the same inode number. They both point to the same file therefore they are hard links. If `/bin/sh` is a symbolic link to `bash`, then the file associated with `/bin/sh` (inode 938819) contains a path to `/bin/bash`

*Question 6:*

What would `/usr/lib64` contain if it was a symbolic link to the directory `/usr/lib`? Is this possible with a hard link?

*Solution 6:*

If `/usr/lib64` is a symbolic link to `/usr/lib` , then the entry is also a file containing a path to `/usr/lib` This is not possible with a hard link.

*Question 7:*

Name one advantage of symbolic link over hard links and one advantage of hard links over symbolic links.

*Solution 7:*

Symbolic link allow link between folders and across partitions, that is not possible for hard links. Hard link don’t have the problem of broken links, if the file is deleted, and another hard links exists, the link will still be valid. With symbolic links, if the original file (inode) is deleted, all the other link will become “broken”.

*Question 8:*

How many disk operations are needed to fetch the inode for a file with the path name `/usr/ast/courses/os/handout.t`? Assume that the inode for the root directory is in memory, but nothing else along the path is in memory. Also assume that all directories fit in one disk block.

How can we reduce that number of disk operations?
*Solution 8:*

