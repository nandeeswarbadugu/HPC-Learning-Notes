# OpenMP Programming

OpenMP (Open Multi-Processing) is an API for shared-memory parallel programming in C, C++, and Fortran. It is widely used in High Performance Computing (HPC) to exploit multi-core processors within a single node.

OpenMP uses compiler directives, library routines, and environment variables to parallelize code with minimal changes.

---

## 1. Multi-threaded Programming

### What is Multi-threading?

Multi-threading allows a program to execute multiple threads concurrently within a single process. All threads share the same memory space.

### Key Concepts

#### Thread

* Smallest unit of execution
* Shares memory with other threads

#### Fork-Join Model

OpenMP follows a fork-join execution model:

1. Program starts with a single thread (master thread)
2. When a parallel region is encountered, threads are created (fork)
3. Threads execute concurrently
4. Threads synchronize and merge back (join)

---

### Example

```c
#include <omp.h>
#include <stdio.h>

int main() {
    #pragma omp parallel
    {
        printf("Hello from thread %d\n", omp_get_thread_num());
    }
    return 0;
}
```

---

### Environment Variables

* `OMP_NUM_THREADS`: Number of threads
* `OMP_SCHEDULE`: Scheduling policy

---

### Advantages

* Easy to use
* Incremental parallelism
* Shared memory simplifies data access

### Challenges

* Race conditions
* Synchronization overhead
* Limited to single-node systems

---

## 2. Parallel Loops

Loops are the most common targets for parallelization in OpenMP.

---

### Basic Parallel Loop

```c
#pragma omp parallel for
for (int i = 0; i < N; i++) {
    A[i] = B[i] + C[i];
}
```

Each iteration is executed by different threads.

---

### Loop Scheduling

Controls how iterations are distributed among threads.

#### Static Scheduling

```c
#pragma omp parallel for schedule(static)
```

* Equal distribution of iterations
* Low overhead

#### Dynamic Scheduling

```c
#pragma omp parallel for schedule(dynamic)
```

* Threads request work dynamically
* Useful for uneven workloads

#### Guided Scheduling

```c
#pragma omp parallel for schedule(guided)
```

* Decreasing chunk size
* Balances load and overhead

---

### Private and Shared Variables

Control data visibility across threads.

```c
#pragma omp parallel for private(i) shared(A, B)
```

* **private**: Each thread has its own copy
* **shared**: All threads share the same variable

---

### Reduction Clause

Used for parallel reduction operations.

```c
#pragma omp parallel for reduction(+:sum)
for (int i = 0; i < N; i++) {
    sum += A[i];
}
```

---

### Benefits

* Simple parallelization
* Efficient for data-parallel tasks

---

## 3. Synchronization Techniques

When multiple threads access shared data, synchronization ensures correctness.

---

### Critical Section

Ensures only one thread executes a block at a time.

```c
#pragma omp critical
{
    sum += value;
}
```

---

### Atomic Operations

Optimized version of critical section for simple operations.

```c
#pragma omp atomic
sum += value;
```

---

### Barrier

All threads wait until everyone reaches the barrier.

```c
#pragma omp barrier
```

---

### Single

Only one thread executes a block.

```c
#pragma omp single
{
    printf("Executed by one thread\n");
}
```

---

### Master

Executed only by the master thread.

```c
#pragma omp master
{
    printf("Master thread only\n");
}
```

---

### Locks

Manual control over synchronization.

```c
omp_lock_t lock;
omp_init_lock(&lock);

omp_set_lock(&lock);
// critical section
omp_unset_lock(&lock);

omp_destroy_lock(&lock);
```

---

### Race Conditions

Occurs when multiple threads update shared data without proper synchronization.

---

### Deadlocks

Occurs when threads wait indefinitely for resources.

---

## 4. Writing Efficient OpenMP Programs

---

### Minimize Synchronization

* Avoid unnecessary critical sections
* Use reductions when possible

---

### Optimize Data Locality

* Use cache-friendly data structures
* Avoid false sharing

---

### Load Balancing

* Choose appropriate scheduling policy

---

### Avoid False Sharing

Occurs when threads modify variables in the same cache line.

---

### Example: Parallel Sum

```c
int sum = 0;

#pragma omp parallel for reduction(+:sum)
for (int i = 0; i < N; i++) {
    sum += A[i];
}
```

---

## Summary

OpenMP enables efficient shared-memory parallel programming:

* Multi-threading within a node
* Parallel loops for data processing
* Synchronization techniques for correctness

It is widely used in HPC for exploiting multi-core CPUs.


