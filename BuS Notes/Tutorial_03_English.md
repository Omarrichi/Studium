30.05.2024 - Omar Richi (omar.richi@rwth-aachen.de)

*Basics:* 

**file management:**
1. File abstraction
2. Directory abstraction
3. File systems
4. Caching

**Need for Abstraction**
- Every device work differently
- Can't expose directly to users

Main Abstraction for users $\Rightarrow$ files
OS component to manage files $\Rightarrow$ file System

---
**File:**
- name
- structure
- types
- attributes
- operations
- descriptors

**File Types: **
- Regular files
- Directories

**File Operations: **
- Create
- Delete
- Get/Set attributes
- Rename
- Open: Returns a *file descriptor*
- Close
- Read/Write
- Seek

**File descriptor:**
- state of currently open file
- multi open == multi file descriptors

![[Pasted image 20240530203736.png]]

**Paths:**
- Absolute: starts from the root of the system up to the file
- relativ: starts from the current location, the current working directory (cwd) 

**Hard Links and Symbolic Links:**
- Hard Link: Copy the file $\Rightarrow$ a file can appear in multiple directories
- Symbolic Link: special file containing a path to another file
	- when Symbolic Link is deleted it does not delete the file it is pointing at

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
- Total storage from direct entries = $10 \times 4096$ bytes

A single indirect entry points to a block that contains disk addresses of other data blocks.

- Number of addresses in a single block = $\frac{4096 \text{bytes}}{4 \text{ bytes/address}}=1024$

- Each of these 1024 addresses points to a 4 KiB block.

- Total storage from a single indirect entry = - $1024 \times 4096$ bytes.

- $1024^2 * 4$ KiB for the double indirect block
- $1024^3 * 4$ KiB for the triple indirect block

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

- 1 disk IO for fetching the block directory for `/` (inside the block, we get the inode number of `usr`)
- 1 disk IO for fetching the inode of `/usr`
- 1 disk IO for fetching the block directory for `/usr` (we get the inode number of `ast`)
- 1 disk IO for fetching the inode of `/usr/ast`
- 1 disk IO for fetching the block directory for `/usr/ast`
- 1 disk IO for fetching the inode of `/usr/ast/courses`
- 1 disk IO for fetching the block directory for `/usr/ast/courses`
- 1 disk IO for fetching the inode of `/usr/ast/courses/os`
- 1 disk IO for fetching the block directory for `/usr/ast/courses/os`
- 1 disk IO for fetching the inode of `/usr/ast/courses/os/handout.t`

10 disk accesses in total. To reduce that number (and because disk IO is very expensive), modern operating systems usually have an associative cache between the directory entries and the inode number

## Basic file management:

In this exercise, you will learn how to:
1. read the manual pages for system calls
2. access files at low level

### 1. Read the manual:
Read and understand the `open` syscall on the manual, what are the arguments, what is the return value?

https://man7.org/linux/man-pages/man2/open.2.html

How will you call this function to create a new file? What does the flag `O_TRUNCATE` is doing? How to combine multiple flags?

*Solution:*

The arguments are:
- a `string` that represent the pathname of the file
- a `int` that represent flags
- an optional argument `mode` used when creating a new file

On success, the function returns the new file descriptor, on error, -1 is returned

Creating a new file can be made using the flag `O_CREAT`, and we need to add the mode (refer to UNIX permissions on the manual of `open()`).

```c
1 int fd =  open("new_file", O_CREAT, 00700);
```

The flag `O_TRUNCATE` is used to truncate a file upon open, which discards all data inside the file.

Finally, to combine multiple flags, we use the OR operator. for example, to write to a file and discord the current data:

```c
1 int fd = open("file", O_RDWR | O_TRUNCATE);
```

### 2. Open and modify a file

Write a C code that opens a file, that modifies all the occurrences of the char `'a'` to `'b'`, starting at the offset 592, until the end of the file.

*Solution:*

```c
int main() {
	int fd = open("file", O_RDWR);
	if (fd < 0) {
		perror("open");
		exit(1);
	}

	int ret = lseek(fd, 592, SEEK_SET);
	if (ret == -1) {
		perror("lseek");
		goto out;
	}

	char buf;

	do {
		ret = read(fd, &buf, 1);
		if (ret != 1) {
			goto out;
		}
		if (buf == 'a') {
			buf = 'b';
			if (lseek(fd, -1, SEEK_CUR) == -1) {
				perror("lseek\n");
				exit(1);
			}
			ret = write(fd, &buf, 1);
		}
	} while(ret == 1);

out:
	close(fd);
}
```

##### **Detailed Explanation:**

**1. Open File:**

```c
int fd = open("file", O_RDWR);
if (fd < 0) {
    perror("open");
    exit(1);
}
```

- The `open` function is used to open the file named "file" with read and write permissions (`O_RDWR`).
- If the file cannot be opened (`fd < 0`), `perror` prints an error message, and `exit(1)` terminates the program.

**2. Seek to a specific position:**

```c
int ret = lseek(fd, 592, SEEK_SET);
if (ret == -1) {
    perror("lseek");
    goto out;
}
```

- The `lseek` function sets the file offset to byte 592 from the beginning of the file (`SEEK_SET`).
- If the seek operation fails (`ret == -1`), `perror` prints an error message, and the program jumps to the `out` label to close the file and exit.

**3. Read and modify file content**

```c
char buf;

do {
    ret = read(fd, &buf, 1);
    if (ret != 1) {
        goto out;
    }
    if (buf == 'a') {
        buf = 'b';
        if (lseek(fd, -1, SEEK_CUR) == -1) {
            perror("lseek\n");
            exit(1);
        }
        ret = write(fd, &buf, 1);
    }
} while (ret == 1);
```

- A `char buf` is declared to store the byte read from the file.
- The `do-while` loop continuously reads one byte at a time from the file into `buf`.
- If `read` does not return 1 (indicating end-of-file or an error), the program jumps to the `out` label.
- If the byte read is `'a'`, it is changed to `'b'`.
- The `lseek(fd, -1, SEEK_CUR)` moves the file offset one byte back, so the just-read byte can be overwritten.
- If `lseek` fails, an error message is printed and the program exits.
- The `write` function writes the modified byte (`'b'`) back to the file.
- The loop continues until `read` does not return 1, indicating no more bytes are read (end-of-file or error).

**4. Cleanup and exit**

```c
out:
close(fd);
return 0;
```

- The `out` label is used to close the file descriptor `fd` and return 0, ending the program.
- This ensures that the file is closed properly whether the program finishes normally or encounters an error.


**Summary:**

This program opens a file named "file" in read-write mode, seeks to byte offset 592, and then reads the file byte-by-byte from that point. If it encounters the character `'a'`, it changes it to `'b'`, writes the modified byte back to the same position, and continues reading. This continues until the end of the file or an error occurs. The file is properly closed before the program exits to ensure resource cleanup.

### Understanding relations between file descriptor, file table and inodes

In this task, we'll be analyzing the state of file descriptors, file structures, and inodes in a parent and child process at specific points in the program execution

```c
int main()
{
	char buffer[16];

	int fd0 = open("file1.txt", O_RDONLY);
	int fd1 = open("file2.txt", O_RDWR);
	int fd2 = open("file3.txt", O_RDONLY, O_TRUNCATE);
	int fd3 = dup(fd2);

	lseek(fd0, 19, SEEK_SET);
	lseek(fd1, 122, SEEK_SET);
	lseek(fd2, 5, SEEK_SET);
	read(fd3, buffer, 16);

	if (fork() == 0) {
		close(fd0);
		close(fd2);

		int fd4 = open("file5.txt", O_RDONLY);

		lseek(fd4, 12, SEEK_SET);
		write(fd1, BUFFER, 16);
		lseek(fd3, -21, SEEK_CUR);

		close(fd1);
		close(fd3);
		close(fd4);
		return;
	}

	close(fd0);
	close(fd1);
	close(fd2);
	close(fd3);
}
```

### Detailed Explanation:




**1. Variable Declarations:**
```c
char buffer[16];
```
- Declares a buffer of 16 characters to hold data read from a file.

**2. Opening Files: **

```c
int fd0 = open("file1.txt", O_RDONLY);
int fd1 = open("file2.txt", O_RDWR);
int fd2 = open("file3.txt", O_RDONLY, O_TRUNCATE);
int fd3 = dup(fd2);
```

- `fd0` opens `file1.txt` in read-only mode.
- `fd1` opens `file2.txt` in read-write mode.
- `fd2` attempts to open `file3.txt` in read-only mode, but `O_TRUNCATE` is invalid here since it's used for write mode; this call might fail or be ignored.
- `fd3` duplicates `fd2`, so `fd3` is another file descriptor for `file3.txt`.

**3. Seeking in Files:**

```c
lseek(fd0, 19, SEEK_SET);
lseek(fd1, 122, SEEK_SET);
lseek(fd2, 5, SEEK_SET);
```

- `lseek` changes the file offset for the file descriptors.
- `fd0`'s offset is moved to byte 19.
- `fd1`'s offset is moved to byte 122.
- `fd2`'s offset is moved to byte 5.

**4. Reading from File:**

```c
read(fd3, buffer, 16);
```
- Reads 16 bytes from `fd3` (which is `file3.txt` at offset 5) into `buffer`.

**5. Forking a Child Process:**

```c
if (fork() == 0) {
```

- `fork()` creates a new process. If `fork()` returns 0, it's the child process.

**6. Child Process Execution:**

```c
close(fd0);
close(fd2);
```

- Child process closes `fd0` and `fd2`.

```c
int fd4 = open("file5.txt", O_RDONLY);
lseek(fd4, 12, SEEK_SET);
write(fd1, buffer, 16);
lseek(fd3, -21, SEEK_CUR);
```

- Opens `file5.txt` in read-only mode as `fd4`.
- Seeks to byte 12 in `file5.txt`.
- Writes the 16 bytes from `buffer` to `fd1` (which is `file2.txt` at offset 122).
- Moves the offset of `fd3` (still `file3.txt`) back by 21 bytes from the current position.

```c
close(fd1);
close(fd3);
close(fd4);
return;
```

- Closes `fd1`, `fd3`, and `fd4`.
- Child process exits

**7. Parent Process Execution:**

```c
close(fd0);
close(fd1);
close(fd2);
close(fd3);
```

- Parent process closes all file descriptors: `fd0`, `fd1`, `fd2`, and `fd3`.

Line 14: 

![[Pasted image 20240530221149.png]]

Line 24:

![[Pasted image 20240530221236.png]]


**Questions:**

![[Pasted image 20240530222635.png]]