28.07.2024 - Omar Richi (omar.richi@rwth-aachen.de)

## Task 1: Semaphore

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
struct Barrier {
    int count;
    int n_threads;
    sem_t mutex;
    sem_t barrier;
};

// initialize the barrier for later use
void barrier_init(struct Barrier *barrier, int n_threads) {
    barrier->count = 0;
    barrier->n_threads = n_threads;
    sem_init(&barrier->mutex, 0, 1); // Mutex semaphore initially unlocked
    sem_init(&barrier->barrier, 0, 0); // Initially locked
}

// Barrier to ensure all threads wait here before being released
void barrier_wait(struct Barrier *barrier) {
    sem_wait(&barrier->mutex); // Take the mutex
    barrier->count++; // Only 1 thread can increase the shared variable at a time

    if (barrier->count == barrier->n_threads) {
        // The last thread calling "wait" will enter this if-condition, unlocking
        // another thread, which will in its turn unlock another thread,
        // until all are unlocked
        sem_post(&barrier->mutex);
        sem_post(&barrier->barrier);
        return;
    }

    sem_post(&barrier->mutex);

    sem_wait(&barrier->barrier);
    sem_post(&barrier->barrier);
}
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
- `sem_init(&barrier->mutex, 0, 1)`: Initializes the `mutex` semaphore to 1, meaning it is initially unlocked.
- `sem_init(&barrier->barrier, 0, 0)`: Initializes the `barrier` semaphore to 0, meaning it is initially locked.

3. **Wait Function:**
```c
void barrier_wait(struct Barrier *barrier) {
    sem_wait(&barrier->mutex); // Take the mutex
    barrier->count++; // Only 1 thread can increase the shared variable at a time

    if (barrier->count == barrier->n_threads) {
        // The last thread calling "wait" will enter this if-condition, unlock
        // one another thread, which will in its turn unlock another thread...
        // until all are unlocked
        sem_post(&barrier->mutex);
        sem_post(&barrier->barrier);
        return;
    }

    sem_post(&barrier->mutex);

    sem_wait(&barrier->barrier);
    sem_post(&barrier->barrier);
}
```

- The `barrier_wait` function is called by each thread when it reaches the barrier.
- `sem_wait(&barrier->mutex)`: Locks the `mutex` semaphore, ensuring exclusive access to the `count` variable.
- `barrier->count++`: Increments the `count` of threads that have reached the barrier.
- `if (barrier->count == barrier->n_threads)`: Checks if all threads have reached the barrier.
	- If true:
		- `sem_post(&barrier->mutex)`: Unlocks the `mutex` semaphore.
		- `sem_post(&barrier->barrier)`: Unlocks the `barrier` semaphore, allowing threads to proceed.
		- `return`: Exits the function.
	- If false:
		- `sem_post(&barrier->mutex)`: Unlocks the `mutex` semaphore.
		- `sem_wait(&barrier->barrier)`: Blocks the thread on the `barrier` semaphore until it is unlocked by the last thread.
		-  `sem_post(&barrier->barrier)`: Ensures that the next thread can proceed.



## Task 2: Mensa Ahornstraße

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


*Solution:*

#### a)
Busy waiting occurs when a process continuously checks a condition without performing productive work. This results in high CPU usage without accomplishing any useful task.

Two examples from the pseudocode:
1. `while (studentsAtCounter > 0);` : The process repeatedly checks the condition until no other student is at the counter.
2. `while (klassiker == 0);` or `while (vegetarian == 0);` : The process repeatedly checks the condition until the desired dish is available.


#### b)

```c
#include <semaphore.h>

// Constants
#define KLASSIKER 0
#define VEGETARIAN 1

// Shared variables
int studentsAtCounter = 0;
int klassiker = 0;
int vegetarian = 0;

// Mutexes
Mutex counterLock;
Mutex klassikerLock;
Mutex vegetarianLock;

void fillKlassiker(int n) {
    mutex_lock(&klassikerLock);
    klassiker += n;
    mutex_unlock(&klassikerLock);
}

void fillVegetarian(int n) {
    mutex_lock(&vegetarianLock);
    vegetarian += n;
    mutex_unlock(&vegetarianLock);
}

void arrivalStudent(int wish) {
    // Ensure only 1 student at a time
    mutex_lock(&counterLock);
    studentsAtCounter++;

    if (wish == KLASSIKER) {
        while (1) {
            mutex_lock(&klassikerLock);
            if (klassiker > 0) {
                klassiker--;
                mutex_unlock(&klassikerLock);
                break;
            }
            mutex_unlock(&klassikerLock);
        }
    } else if (wish == VEGETARIAN) {
        while (1) {
            mutex_lock(&vegetarianLock);
            if (vegetarian > 0) {
                vegetarian--;
                mutex_unlock(&vegetarianLock);
                break;
            }
            mutex_unlock(&vegetarianLock);
        }
    }

    studentsAtCounter--;
    mutex_unlock(&counterLock);
}
```

*Details:*

- **Constants**: Defines `KLASSIKER` and `VEGETARIAN` with values `0` and `1`, respectively.
- **Shared Variables**:
	- `studentsAtCounter`: Tracks the number of students at the counter.
	- `klassiker` and `vegetarian`: Track the number of meals available for each type.
- **Mutexes**: Defines three mutexes for controlling access to shared variables.


**Functions:**
- `fillKlassiker`: Adds `n` meals to `klassiker`. Uses a mutex to ensure only one thread can update the `klassiker` variable at a time.
- `fillVegetarian`: Adds `n` meals to `vegetarian`. Uses a mutex to ensure only one thread can update the `vegetarian` variable at a time.
- `arrivalStudent`:
	- Locks the `counterLock` mutex to ensure only one student is processed at a time.
	- Depending on the student's meal choice (`wish`), it locks the corresponding meal mutex (`klassikerLock` or `vegetarianLock`).
	- The student waits if no meals of their choice are available.
	- Decrements the meal count when a meal is taken.
	- Unlocks the corresponding mutex and then the `counterLock` mutex once the student is processed.

This code ensures that meal counts are accurately managed and that only one student can take a meal or be at the counter at a time, preventing race conditions.

Notice that this solution still relies on busy waiting, but no more race condition is possible thanks to mutexes.

#### c)

```c
#include <semaphore.h>

// Constants
#define KLASSIKER 0
#define VEGETARIAN 1

// Semaphores
Semaphore klassikerSem(0); // initialize semaphore to 0: no klassiker available at start
Semaphore vegetarianSem(0); // initialize semaphore to 0: no vegetarian available at start
Semaphore counter(1); // initialize semaphore to 1: one place available at the counter

void fillKlassiker(int n) {
    for (int i = 0; i < n; i++) {
        sem_post(&klassikerSem);
    }
}

void fillVegetarian(int n) {
    for (int i = 0; i < n; i++) {
        sem_post(&vegetarianSem);
    }
}

void arrivalStudent(int wish) {
    // Ensure only 1 student at a time at the counter
    sem_wait(&counter);
    
    if (wish == KLASSIKER) {
        sem_wait(&klassikerSem);
    } else if (wish == VEGETARIAN) {
        sem_wait(&vegetarianSem);
    }
    
    sem_post(&counter);
}
```

*Details:*
- **Constants**: Defines `KLASSIKER` and `VEGETARIAN` with values `0` and `1`, respectively.
- **Semaphores**:
	- `klassikerSem`: Semaphore initialized to `0` indicating no `klassiker` meals available initially.
	- `vegetarianSem`: Semaphore initialized to `0` indicating no `vegetarian` meals available initially.
	- `counter`: Semaphore initialized to `1` indicating one place available at the counter.

**Functions:**
- `fillKlassiker(int n)`:
	- Increases the `klassikerSem` semaphore by `n`. This function is used to add `n` Klassiker meals to the available pool.
- `fillVegetarian(int n)`:
	- Increases the `vegetarianSem` semaphore by `n`. This function is used to add `n` vegetarian meals to the available pool.
- `arrivalStudent(int wish)`:
	- Ensures mutual exclusion among students accessing the counter using the `counter` semaphore.
	- Depending on the student's meal choice (`wish`), it waits on the corresponding semaphore (`klassikerSem` or `vegetarianSem`) to ensure a meal of the desired type is available.
	- After acquiring a meal, it releases the `counter` semaphore to allow other students to access the counter.

This code manages meal availability using semaphores to synchronize access among multiple threads or processes, ensuring that students wait when their desired meal type is unavailable and allowing only one student at a time to access the counter.


## Task 3: Deadlock

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

*Solution:*
Deadlock can occur if all three processes execute their first wait.

- Process A takes hardDrive
- Process B takes printer
- Process C takes cardReader
- Process A is waiting on cardReader
- Process B is waiting on hardDrive
- Process C is waiting on printer


One solution is to define an order to take the locks. The order should be respected by all processes. For instance, hardDrive -> printer -> cardReader.

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
    sem_wait(hardDrive);
    sem_wait(printer);
    useDevices();
    sem_post(hardDrive);
    sem_post(printer);
}
```

**Process C:**

```c
while (condition) {
    sem_wait(printer);
    sem_wait(cardReader);
    useDevices();
    sem_post(printer);
    sem_post(cardReader);
}
```

## Task 4:

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


*Solution:*

**Constants:**

```c
struct barbershop {
    int chairs = 20; // Total number of chairs in the barbershop
    int waiting;     // Number of people currently waiting
    sem_t customers; // Semaphore for customers, initialized to 0
    sem_t barbers;   // Semaphore for barbers, initialized to 0
    sem_t mutex;     // Mutex semaphore, initialized to 1
};
```

*Details:*
- `chairs`: This variable represents the total number of chairs available in the barbershop waiting area. In this case, there are 20 chairs.
- `waiting`: This variable keeps track of the number of customers currently waiting in the barbershop.
- `sem_t customers`: This semaphore is used to control access to the customers waiting in the barbershop. It is initialized to 0, indicating initially there are no customers.
- `sem_t barbers`: This semaphore controls access to the barbers available in the shop. Like `customers`, it is also initialized to 0, indicating initially there are no barbers available.
- `sem_t mutex`: This semaphore (`mutex`) serves as a mutual exclusion lock that ensures only one thread can access the critical sections of the `barbershop` structure at any given time. It is initialized to 1, allowing one thread to enter the critical section initially.