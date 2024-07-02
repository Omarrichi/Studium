28.07.2024 - Omar Richi (omar.richi@rwth-aachen.de)

### Task 1: Semaphore

Using semaphore, implement the following synchronization mechanisms.

1. Mutex

Define a structure  `struct Mutex` and implement the following functions:

```c
1
2 void mutex_init(struct Mutex *mutex);
3
4 void mutex_lock(struct Mutex *mutex);
5
6 void mutex_unlock(struct Mutex *mutex);
```

1. Barrier

Define a structure `struct Barrier` and implement the following functions:

```c
1
2 void barrier_init(struct barrier *barrier, int n_threads);
3
4 void mutex_wait(struct Mutex *mutex);
5
```