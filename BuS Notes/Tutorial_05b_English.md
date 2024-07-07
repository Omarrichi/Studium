05.07.2024 - Omar Richi (omar.richi@rwth-aachen.de)

# Inter-process-communication

**Basics for Pipes and Shared memory**

Pipes:
-  Allow data transfer from one process to another
- bidirectional
- Anonymous pipes: mainly between parent and child process
- Named pipes:
	- Communication between unrelated process and support bidirectional communcation
Usage: 
- Simple data transfer between processes
- act like data stream
	- one process


## Introduction Questions

**TASK 1 Code**

receiver.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <unistd.h>

#define BUFFER_SIZE 256

int main() {
    char mypipe[] = "/tmp/mypipe";
    int fd_recv = open(mypipe, O_RDONLY);
    if (fd_recv == -1) {
        perror("open");
        exit(1);
    }
    char buffer_recv[BUFFER_SIZE];
    do {
        if (read(fd_recv, buffer_recv, sizeof(buffer_recv)) == -1) {
            perror("read");
            exit(1);
        }
        printf("> %s", buffer_recv);
    } while (strcmp(buffer_recv, "exit\n"));
    if (close(fd_recv) == -1) {
        perror("close");
        exit(1);
    }
    return 0;
}
```

### Explanation

1. **Includes and Defines**:
    
    - The program includes necessary headers for I/O operations, memory management, and inter-process communication (IPC).
    - `BUFFER_SIZE` is defined as 256, which sets the maximum size for the buffer that will store incoming messages.
2. **Main Function**:
    
    - `char mypipe[] = "/tmp/mypipe";`: This specifies the path to the named pipe (FIFO) used for communication.
    - `int fd_recv = open(mypipe, O_RDONLY);`: Opens the named pipe for reading. The `O_RDONLY` flag indicates that the pipe is opened in read-only mode.
    - **Error Handling**:
        - If `open` fails, `perror("open")` prints an error message and `exit(1)` terminates the program.
    - `char buffer_recv[BUFFER_SIZE];`: Declares a buffer to store the data read from the named pipe.
    - **Reading from the Pipe**:
        - A `do-while` loop continuously reads data from the named pipe using `read(fd_recv, buffer_recv, sizeof(buffer_recv))`.
        - If `read` fails, an error message is printed and the program exits.
        - `printf("> %s", buffer_recv);` prints the received message to the console.
    - **Termination Condition**:
        - The loop continues until the received message matches "exit\n".
    - **Closing the Pipe**:
        - After exiting the loop, `close(fd_recv)` closes the file descriptor associated with the named pipe.
        - If `close` fails, an error message is printed and the program exits.

sender.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <unistd.h>

#define BUFFER_SIZE 256

int main() {
    char mypipe[] = "/tmp/mypipe";
    if (mkfifo(mypipe, S_IRUSR | S_IWUSR) == -1) {
        perror("mkfifo");
        exit(1);
    }
    int fd_send = open(mypipe, O_WRONLY);
    if (fd_send == -1) {
        perror("open");
        exit(1);
    }
    char buffer_send[BUFFER_SIZE];
    do {
        fgets(buffer_send, BUFFER_SIZE, stdin);
        if (write(fd_send, buffer_send, strlen(buffer_send) + 1) == -1) {
            perror("write");
            exit(1);
        }
    } while (strcmp(buffer_send, "exit\n"));
    if (remove(mypipe) == -1) {
        perror("remove");
        exit(1);
    }
    if (close(fd_send) == -1) {
        perror("close");
        exit(1);
    }
    return 0;
}
```


### Explanation

1. **Includes and Defines**:
    
    - The program includes necessary headers for I/O operations, memory management, and inter-process communication (IPC).
    - `BUFFER_SIZE` is defined as 256, which sets the maximum size for the buffer that will store outgoing messages.
2. **Main Function**:
    
    - `char mypipe[] = "/tmp/mypipe";`: Specifies the path to the named pipe (FIFO) used for communication.
    - `if (mkfifo(mypipe, S_IRUSR | S_IWUSR) == -1)`: Creates the named pipe with read and write permissions for the user. If `mkfifo` fails (e.g., if the pipe already exists), an error message is printed and the program exits.
    - `int fd_send = open(mypipe, O_WRONLY);`: Opens the named pipe for writing. The `O_WRONLY` flag indicates that the pipe is opened in write-only mode.
    - **Error Handling**:
        - If `open` fails, `perror("open")` prints an error message and `exit(1)` terminates the program.
    - `char buffer_send[BUFFER_SIZE];`: Declares a buffer to store the data to be sent to the named pipe.
    - **Writing to the Pipe**:
        - A `do-while` loop continuously reads input from the user using `fgets(buffer_send, BUFFER_SIZE, stdin)`.
        - The input is then written to the named pipe using `write(fd_send, buffer_send, strlen(buffer_send) + 1)`. The `+1` ensures that the null terminator is also written.
        - If `write` fails, an error message is printed and the program exits.
    - **Termination Condition**:
        - The loop continues until the user inputs "exit\n".
    - **Removing the Pipe**:
        - After exiting the loop, `remove(mypipe)` deletes the named pipe.
        - If `remove` fails, an error message is printed and the program exits.
    - **Closing the Pipe**:
        - Finally, `close(fd_send)` closes the file descriptor associated with the named pipe.
        - If `close` fails, an error message is printed and the program exits.


**TASK 2 Code**

```c

#include <semaphore.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/mman.h>
#include <sys/wait.h>
#include <time.h>
#include <unistd.h>

struct Stack {
  int idx;
  int size;
  sem_t sem_free;
  sem_t sem_used;
  sem_t mutex;
  int *stack;
};

struct Stack *init_stack(int size) {
  struct Stack *stack = mmap(NULL, sizeof(struct Stack), PROT_READ | PROT_WRITE,
                             MAP_SHARED | MAP_ANONYMOUS, -1, 0);
  int *stack_t = mmap(NULL, sizeof(int) * size, PROT_READ | PROT_WRITE,
                      MAP_SHARED | MAP_ANONYMOUS, -1, 0);

  if (!stack || !stack_t) {
    perror("mmap");
    exit(1);
  }

  stack->idx = 0;
  stack->size = size;
  sem_init(&stack->sem_free, 1, size);
  sem_init(&stack->sem_used, 1, 0);
  sem_init(&stack->mutex, 1, 1);

  stack->stack = stack_t;
  return stack;
}

int shared_pop(struct Stack *stack) {
  sem_wait(&stack->sem_used);

  sem_wait(&stack->mutex);
  int ret = stack->stack[--stack->idx];
  sem_post(&stack->mutex);

  sem_post(&stack->sem_free);
  return ret;
}

void shared_push(struct Stack *stack, int value) {
  sem_wait(&stack->sem_free);

  sem_wait(&stack->mutex);
  stack->stack[stack->idx++] = value;
  sem_post(&stack->mutex);

  sem_post(&stack->sem_used);
}

int main() {
  srand(time(NULL));

  struct Stack *stk = init_stack(200);

  for (int i = 0; i < 10; i++) {
    if (fork() == 0) {
      while (1) {
        shared_push(stk, rand() % 20);
      }
    }
  }

  for (int i = 0; i < 10; i++) {
    if (fork() == 0) {
      while (1) {
        shared_pop(stk);
      }
    }
  }

  wait(NULL);
}

```

### Explanation

- **Includes**: The program includes several standard libraries for semaphore operations, memory management, process control, and random number generation.
- **Struct Stack**: This structure represents the stack with the following fields:
    - `idx`: Current index or top of the stack.
    - `size`: Maximum size of the stack.
    - `sem_free`: Semaphore to track the number of free slots in the stack.
    - `sem_used`: Semaphore to track the number of used slots in the stack.
    - `mutex`: Semaphore for mutual exclusion to protect critical sections.
    - `stack`: Pointer to the stack array.


### Stack Initialization

- **mmap**: Allocates shared memory for the stack structure and the stack array using `mmap`. The memory is accessible to all processes.
- **sem_init**: Initializes semaphores:
    - `sem_free` is initialized to the stack size, indicating the number of free slots.
    - `sem_used` is initialized to 0, indicating no slots are used initially.
    - `mutex` is initialized to 1, allowing only one process to access critical sections at a time.
- **Return**: Returns a pointer to the initialized stack.

### Pop Operation

- **sem_wait**: Waits for the `sem_used` semaphore, ensuring there are items to pop.
- **Critical Section**:
    - **sem_wait(&stack->mutex)**: Locks the critical section.
    - **Pop Operation**: Decreases the index and retrieves the value from the stack.
    - **sem_post(&stack->mutex)**: Unlocks the critical section.
- **sem_post(&stack->sem_free)**: Signals that a slot is now free.
- **Return**: Returns the popped value.

### Push Operation

- **sem_wait**: Waits for the `sem_free` semaphore, ensuring there is space to push.
- **Critical Section**:
    - **sem_wait(&stack->mutex)**: Locks the critical section.
    - **Push Operation**: Inserts the value into the stack and increases the index.
    - **sem_post(&stack->mutex)**: Unlocks the critical section.
- **sem_post(&stack->sem_used)**: Signals that a slot is now used.

### Main Function
- **srand(time(NULL))**: Seeds the random number generator with the current time.
- **init_stack(200)**: Initializes the stack with a size of 200.
- **Forking**:
    - Creates 10 child processes that continuously push random values (0-19) onto the stack.
    - Creates 10 child processes that continuously pop values from the stack.
- **wait(NULL)**: Waits for all child processes to finish. However, since the child processes run infinite loops, this program will never exit normally.