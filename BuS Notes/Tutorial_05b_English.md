05.07.2024 - Omar Richi (omar.richi@rwth-aachen.de)

## Introduction Questions

**TASK 2 Code**

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