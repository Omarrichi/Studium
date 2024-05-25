10.07.2024 - Omar Richi (omar.richi@rwth-aachen.de)

```ad-note
title: Allgemeines
Die Notizen ähnlich zum Tutorium sein, mit Grundlagen gefolgt von den Aufgaben.

Ich werde versuchen, für jedes Tutorium-Blatt zwei Versionen zu machen, auf Deutsch und auf Englisch. 

Ich habe mich auch entscheiden einen Discord Server zu erstellen. Das Beitreten ist natürlich freiwillig, es ist nur ein Angebot, denn dort erreicht ihr mich etwas schneller als per Mail.
https://discord.gg/97jDeFcXxZ
```

### Question 1: 

Consider the following piece of C code:

```c
int main() {
	fork();
	fork();
	exit();
}
```

How many processes are created during the execution of this program?

*Grundlagen:* 
- die 5 wichtigen Syscalls sind: 
- read, write, open, close, fork
- Allgemein zur fork:
	- Fork erzeugt einen Kopieprozess aus einem Vaterprozess, beide Prozesse sind zunächst identisch. Die Kopie wird Kindprozess genannt und der originale Prozess wird Vaterprozess genannt.
	- Jeder Prozess hat einen eindeutigen PID, damit können wir aber auch zwischen Vater- und Kindprozess unterscheiden
	- Beim Ausführen von Fork, bekommt man einen Resultat zurück (einen PID zurück).
		- Falls dieser Resultat $-1$ ist, dann ist fork fehlgeschlagen
		- Falls diese $> 0$ ist, dann ist der Resultat, die PID von dem Vater prozess, und er hat den Resultat zurückgegeben 
		- Falls diese $= 0$ ist, dann ist das Kindprozess angesprochen
	- Durch diese Unterscheidung, kann man mit Anfragen nach fork Resultat, Anweisungen nur für das Kind bzw. nur für den Vater geben

*Lösung:*
```mermaid
graph TD;
    ((fork1))-->fork2;
    fork1-->fork2`;
    

```
### Question 2:

In a non-preemptive batch system, there are four jobs waiting to be executed with expected run times: A(9), B(6), C(3), D(5). Which scheduling algorithm should be used to minimize the average response time (latency)? What would be the optimal order for running these jobs?

### Question 3:

Suppose we are operating a preemptive system using the algorithm identified in the previous question. At time t=10, two new processes arrive, E(2) and F(7). what is the current state of the processes? Which scheduling algorithm should be employed now to minimize the average response time? What will be the sequence of execution  starting from t=10 to achieve this goal?


### Question 4:

Why in addition to the states $ready$ and $running$, is there another state called $blocked$? What is achieved by this state?

Using the top program in the Linux shell, you can view the processes currently managed by the kernel. It provides a real-time view, including current information about process state, CPU usage, and memory usage. On a typical Linux system, you’ll notice that a large number of processes are in the sleeping state (corresponds to blocked based on our definition). This does not represent the ready state - which is combined with the running state in top. What do you suspect: why are so many processes in the blocked state?

## Tasks

### Task 2.1 Process and Process Creation

write a simple C program following these specifications:

- Create and initialize a global variable to 0
- Inside a loop, each iteration will:
	- increment the global variable
	- Call ```fork()```
	- The child should print "Child" followed by its $pid$ and the value of the global variable 
	- The parent should print “Parent” followed by its $pid$ and the value of the global variable
	- When the global variable reaches 5, exit the loop and exit the program
- Ensure that each parent and child processes continue to execute the loop until the variable reaches 5

*Question*: Observe the output, how many processes are created by the end of execution? What happens if the loop doesn't stop so early?

### Task 2.2 Thread global variable increment

Modify the program from Task 2.1 to use threads instead of fork(). Use the pthread API (```pthread_create```, ```pthread_exit```,```pthread_join```).

*Question*: how many threads are created in total? Why is it different from the ```fork``` version

### Task 2.3 Exec Syscall

Write a C program that performs a ```fork()```. The child process should execute the ```ls /``` command using ```execl()```, while the parent waits for the child to finish and then prints a completion message.

Here is the example how to use the ```execl``` syscall:

```c
int execl(const char *pathname, const char *arg, ...
/*, (char *) NULL */);
```

```c
// note that:
// - the argument after the executable should contain the 
// - the last argument should be NULL
int ret = execl("/bin/ls","ls", "/", NULL);
```

Write a second C program that has the same behavior, except that it will create a new thread instead of a fork. The new thread will be calling the ```ls /``` command with ```execl()```. The main thread should be waiting for the child completion and print an exit message.

*Questions*: What differences do you observe between using ```fork()``` and creating a new thread (```pthread_create()```) in this context?

