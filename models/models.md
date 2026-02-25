# Parallel Computing Models

Parallel computing enables multiple computations to be performed simultaneously, significantly improving performance for large-scale and complex problems. This document explains key parallel computing models used in High Performance Computing (HPC).

---

## 1. Shared Memory vs Distributed Memory

### Shared Memory Architecture

In a shared memory system, all processors access a common memory space.

### Characteristics

* All cores can read/write to the same memory
* Communication happens via shared variables
* Easier to program

### Example Technologies

* OpenMP
* POSIX Threads (pthreads)

### Advantages

* Simple programming model
* Fast communication (no network overhead)

### Disadvantages

* Limited scalability (usually within a single node)
* Risk of race conditions
* Requires synchronization (locks, mutexes)

---

### Distributed Memory Architecture

In distributed memory systems, each processor (node) has its own private memory.

### Characteristics

* Nodes communicate via a network
* No shared global memory
* Explicit communication required

### Example Technologies

* MPI (Message Passing Interface)

### Advantages

* Highly scalable (thousands of nodes)
* Suitable for large problems

### Disadvantages

* Complex programming
* Communication overhead

---

### Comparison Table

| Feature       | Shared Memory | Distributed Memory |
| ------------- | ------------- | ------------------ |
| Memory        | Global        | Local per node     |
| Communication | Implicit      | Explicit           |
| Scalability   | Limited       | Very High          |
| Complexity    | Easier        | Harder             |
| Example       | OpenMP        | MPI                |

---

## 2. Multi-threading Concepts

### What is Multi-threading?

Multi-threading allows multiple threads within a single process to execute concurrently, sharing the same memory space.

### Key Concepts

#### Thread

The smallest unit of execution within a process.

#### Concurrency vs Parallelism

* **Concurrency**: Multiple tasks making progress (not necessarily simultaneously)
* **Parallelism**: Multiple tasks executing at the same time

#### Context Switching

The CPU switches between threads to give the illusion of simultaneous execution.

---

### Synchronization Mechanisms

When multiple threads access shared data, synchronization is required.

#### Common Tools

* Mutex (Mutual Exclusion)
* Locks
* Semaphores
* Barriers

### Race Condition

Occurs when multiple threads modify shared data simultaneously, leading to unpredictable results.

---

### Example (OpenMP)

```c
#pragma omp parallel for
for (int i = 0; i < N; i++) {
    A[i] = B[i] + C[i];
}
```

---

### Benefits

* Efficient CPU utilization
* Faster execution
* Lightweight compared to processes

### Challenges

* Debugging complexity
* Synchronization overhead
* Deadlocks

---

## 3. Data Parallelism vs Task Parallelism

Parallelism can be categorized based on how work is divided.

---

### Data Parallelism

In data parallelism, the same operation is applied to different chunks of data in parallel.

### Characteristics

* Same operation on multiple data elements
* Common in scientific computing

### Example

```python
for i in range(N):
    C[i] = A[i] + B[i]
```

This loop can be split across multiple processors.

### Use Cases

* Matrix operations
* Image processing
* Machine learning

### Advantages

* Easy to scale
* Efficient for large datasets

---

### Task Parallelism

In task parallelism, different tasks are executed in parallel.

### Characteristics

* Different operations run simultaneously
* Tasks may operate on same or different data

### Example

* Task 1: Read data
* Task 2: Process data
* Task 3: Write results

---

### Use Cases

* Web servers
* Workflow pipelines
* Simulation pipelines

### Advantages

* Flexible
* Suitable for complex workflows

---

### Comparison Table

| Feature     | Data Parallelism | Task Parallelism     |
| ----------- | ---------------- | -------------------- |
| Focus       | Data             | Tasks                |
| Operation   | Same operation   | Different operations |
| Scalability | High             | Depends on tasks     |
| Complexity  | Lower            | Higher               |

---

## 4. Hybrid Parallel Programming

Modern HPC applications often combine multiple models:

* MPI (Distributed Memory) across nodes
* OpenMP (Shared Memory) within a node

This is called **hybrid parallelism**.

### Example

* Each node runs MPI processes
* Each process uses multiple threads via OpenMP

---

## Summary

Parallel computing models define how computation and communication are structured:

* Shared Memory for intra-node parallelism
* Distributed Memory for inter-node scaling
* Multi-threading for concurrency within processes
* Data Parallelism for large datasets
* Task Parallelism for workflows

Understanding these models is essential for building efficient HPC applications.
