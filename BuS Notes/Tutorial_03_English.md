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
- state of curre