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

The clock interrupt handler on a certain computer requires 2 msec (including process switching overhead) per clock tick. The clock ticks at 60 Hz. What fraction of the CPU is devoted to the clock?

*Question 6*

A computer uses a programmable clock in square-wave mode. If a 500 MHz crystal is used, what should be the value of the holding register to achieve a clock resolution of 

1. a millisecond (a clock tick once every millisecond)?
2. 100 microseconds

*Question 7*

Assuming that it takes 2 nsec to copy a byte, how much time does it take to completely rewrite the screen of an 1920 x 1080 pixel graphics screen with 24-bit color? How many copies can we do per second? with a 60 Hz monitor, how many buffers do we need to avoid screen tearing?


### Tasks


*Task 1*

Disk requests come in to the disk driver for cylinders 10, 22, 20, 2, 40, 6 and 38, in that order. A seek takes 6 msec per cylinder. How much seek time is needed for 

1. First-Come, First-Served (FCFS).
2. Shortest Seek First (SSF).
3. Elevator algorithm (SCAN, initially moving upward).

In all cases, the arm is initally at cylinder 20. There are 40 cylinders in total.


*Task 2*

Assume a Network Interface Controller (NIC) has 5 different buffers. The indes of the buffer currently in use is located in the controller register `nic_buffer_position_reg`.

Consider the following Interrupt Service Routine (ISR) code:

```c
1 index = *nic_buffer_position_reg;
2 // buffer is a local array
3 copy_buffer_from_driver(&kbuf[index], &buffer);
4 schedule_upper_half(&buffer);
5 *nic_buffer_position_reg++;
6 ack_interrupt();
7 return_from_interrupt();
```

What I/O software design goal this interruption service routine violates? Think of a solution to fix it.


*Task 3*

You are using a small computer board, such as a Raspberry Pi, to fetch the weather forecast and display it on a small LCD screen.

1. Which method would you choose to interact with the LCD controller: Programmed I/O, Interrupt Driven I/O, or DMA? Justify your choice.

Now, suppose you add a sensor to your computer. This sensor can read the room temperature and humidity directly from its controller's registers.

1. Which method is best suited if you want to read the values every 5 minutes?
2. Is there a better option if you want to read the values only when they change?
3. How would you approach change if the board were battery-üpwered instead of being plugged into a constant power source?


*Task 4*

A typical sound card is working at 48 kHz with each sample being stored on 16 bits. The sound card controller is using two buffers. While the first buffer is used for playing sounds on the sound card, the other one is being filled by the driver. The drivers is using DMA to copy audio date from the kernel to the audio controller


**Questions**

1. What hardware buffer size should be used if you want to play 10ms audio data at a time?
2. What hardware buffer size should be used if we want to play 100ms audio data?
3. what is the difference in latency and CPU usage between 10ms and 100ms?
4. What is the advantage of using a double buffering technique?
5. How does Direct Memory Access (DMA) improve the efficiency of audio data transfer compared to CPU-driven data transfer?