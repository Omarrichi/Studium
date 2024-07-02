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

2. Barrier

Define a structure `struct Barrier` and implement the following functions:

```c
1 // initialize your struct Barrier
2 void barrier_init(struct barrier *barrier, int n_threads);
3
4 // wait until all the threads are waiting, then release the threads
5 void mutex_wait(struct Mutex *mutex); 
```

*Solution:*

1. Mutex: 

```c
struct Mutex {
    sem_t mutex;
};

void mutex_init(struct Mutex *mutex) {
    sem_init(&mutex->mutex, 0, 1); // Initially unlocked
}

void mutex_lock(struct Mutex *mutex) {
    sem_wait(&mutex->mutex);
}

void mutex_unlock(struct Mutex *mutex) {
    sem_post(&mutex->mutex);
}

```
*Details:*
1. **Struct Definition:**

```c
struct Mutex {
    sem_t mutex;
};
```
- The `Mutex` struct contains a single member, `sem_t mutex`, which is a semaphore

2. **Initialization Function:**

```c
void mutex_init(struct Mutex *mutex) {
    sem_init(&mutex->mutex, 0, 1); // Initially unlocked
}
```
-  The `mutex_init` function initializes the semaphore.
- `sem_init(&mutex->mutex, 0, 1)` initializes the semaphore `mutex->mutex` with an initial value of 1, meaning it is initially unlocked (available).
- The second parameter `0` indicates that the semaphore is shared between threads of the same process.


3. **Lock Function:**

```c
void mutex_lock(struct Mutex *mutex) {
    sem_wait(&mutex->mutex);
}
```

- The `mutex_lock` function locks the mutex.
-  `sem_wait(&mutex->mutex)` decrements the semaphore value. If the semaphore value is greater than zero, the function returns immediately. If the semaphore value is zero, the calling thread is blocked until the semaphore value becomes greater than zero.

4. **Unlock Function:**

```c
void mutex_unlock(struct Mutex *mutex) {
    sem_post(&mutex->mutex);
}
```
- The `mutex_unlock` function unlocks the mutex.
-  `sem_post(&mutex->mutex)` increments the semaphore value. If there are any threads blocked on the semaphore, one of them will be unblocked.

2. Barrier:

```c

code
```

*Details:*

1. **Struct Definition:**

```c
struct Barrier {
    int count;
    int n_threads;
    sem_t mutex;
    sem_t barrier;
};
```

- The `Barrier` struct contains:
	- `count`: Keeps track of how many threads have reached the barrier.
	- `n_threads`: The total number of threads that must reach the barrier before any can proceed.
	- `mutex`: A semaphore used to protect access to the `count` variable.
	-  `barrier`: A semaphore used to block threads at the barrier.

2. **Initialization Function:**

```c
void barrier_init(struct Barrier *barrier, int n_threads) {
    barrier->count = 0;
    barrier->n_threads = n_threads;
    sem_init(&barrier->mutex, 0, 1); // Mutex semaphore initially unlocked
    sem_init(&barrier->barrier, 0, 0); // Initially locked
}
```

- The `barrier_init` function initializes the barrier.
- `barrier->count = 0`: Initializes the count of threads that have reached the barrier to 0.
- `barrier->n_threads = n_threads`: Sets the number of threads that need to reach the barrier.











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

**Process A:**

```c 
while (condition) {
    sem_wait(hardDrive);
    sem_wait(cardReader);
    useDevices();
    sem_post(hardDrive);
    sem_post(cardReader);
}
```

**Process B:**

```c
while (condition) {
    sem_wait(printer);
    sem_wait(hardDrive);
    useDevices();
    sem_post(printer);
    sem_post(hardDrive);
}
```

**Process C:**

```c
while (condition) {
    sem_wait(cardReader);
    sem_wait(printer);
    useDevices();
    sem_post(cardReader);
    sem_post(printer);
}
```

### Task 4:

In this problem, we have a single barber working in a barbershop. The barbershop has a waiting room that can hold up to 20 customers. Only one customer at a time can be in the barber’s chair for a haircut. If there are no customers in the shop, the barber, who often gets tired from working too much, will go to sleep. However, if a customer enters the barbershop and finds the barber sleeping, the customer will wake up the barber.

You need to implement the routines for both the barber and the customers, using proper synchronization mechanisms to ensure the requirements are met.

**Requirements:**
1. The barber should go to sleep if there are no customers waiting.
2. A customer should wake up the barber if they find him sleeping.
3. Only one customer should be in the barber’s chair at a time.
4. The waiting room should not hold more than 20 customers at a time.


**Questions:**
1. Identify the constants that need to be declared.
2. Determine the number of semaphores required and their initial values.
3. Specify the number of mutexes required.

Your implementation should ensure proper synchronization between the barber and the customers, adhering to the constraints mentioned above.