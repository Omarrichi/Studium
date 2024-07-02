28.07.2024 - Omar Richi (omar.richi@rwth-aachen.de)

### Task 1: Semaphore

Using semaphore, implement the following synchronization mechanisms.

1. Mutex

Define a structure  `struct Mutex` and implement the following functions:

```c
1 // initialize your struct Mutex
2 void mutex_init(struct Mutex *mutex);
3
4 void mutex_lock(struct Mutex *mutex);
5
6 void mutex_unlock(struct Mutex *mutex);
```

1. Barrier

Define a structure `struct Barrier` and implement the following functions:

```c
1 // initialize your struct Barrier
2 void barrier_init(struct barrier *barrier, int n_threads);
3
4 // wait until all the threads are waiting, then release the threads
5 void mutex_wait(struct Mutex *mutex); 
```


### Task 2: Mensa Ahornstraße

#### a)

In this task, we simulate the functionality of the Mensa Ahornstraße. We assume there are only two dishes, “Klassiker” and “Vegetarian”. At the counter, both dishes are available. Only one person can be at the counter at a time, stating their food request. If the dish is available, a portion is handed over, the person pays, and then leaves the counter. Otherwise, the person waits at the counter until the dish is available. Whenever the kitchen prepares new portions, they are made available by the cafeteria staff.

Here is the current solution. The `arrivalStudent` function is started by different processes, `fillKlassiker` and `fillVegetarian` are called by only one process.


``` c
#define KLASSIKER 01
#define VEGETARIAN 12

int studentsAtCounter = 0;
int klassiker = 0;
int vegetarian = 0; // shared variables

void fillKlassiker(int n) {
    klassiker += n;
}

void fillVegetarian(int n) {
    vegetarian += n;
}

void arrivalStudent(int wish) {
    while (studentsAtCounter > 0);
    studentsAtCounter++;
    if (wish == KLASSIKER) {
        while (klassiker == 0) {
            // wait
        }
        klassiker--;
    }
    else if (wish == VEGETARIAN) {
        while (vegetarian == 0) {
            // wait
        }
        vegetarian--;
    }
    studentsAtCounter--;
}

```

Explain the term busy waiting in the context of this task and give two examples from the pseudocode.

#### b)

Synchronize the solution using mutex. Justify the placement of the critical sections.

#### c)

Instead of a mutex, synchronize the solution using semaphore. Justify the placement of the critical sections. You may need additional variables for correct synchronization.


### Task 3: Deadlock

Given three resources: hard drive, card reader, and printer. Each process always needs exactly two of these resources exclusively. The following sequences for processes have been defined. All semaphores (hardDrive, cardReader, printer) are mutexes, initialized to 1. Does this solution work? If you find an error or problem, describe how to fix it.

**Process A**