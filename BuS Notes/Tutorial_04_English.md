### Introduction

*Question 1*

In the lecture we discussed that the operation of I/O devices can be controlled by polling or via interrupts. However, in general, the operation through polling is not very efficient. In which cases could it still make sense to use polling? Why?

*Question 2*

Consider the following I/O devices on your computer:

- the mouse
- a hard disk with user files
- a light sensor on smartphone

Decide for each device whether polling or interrupts are more suitable for I/O control and whether it makes sense to use an I/O buffer. Give reasons for your choice.

*Question 3*

Suppose a computer can read and write a memory word in 5 nanoseconds (nsec) Also suppose that when an interrupt occurs, all 32 CPU registers, plus the program counter (PC) and stack pointer (SP) are pushed onto the stack. What is the maximum number of interrupts per second this machine can process?

*Question 4*

In which of the four I/O software layers is each of the following done.

1. Computing the track, sector, and head of for a disk read
2. Writing commands to the device registers.
3. Checking to see if the user is permitted to use the device.
4. Converting binary integers to ASCII for printing.

*Question 5*

The clock interrupt handler on a certain computer requires 2 msec (including process switching overhead)