# MPI (Message Passing Interface)

Message Passing Interface (MPI) is a standardized and portable communication protocol used for parallel programming in distributed memory systems. It is the backbone of most High Performance Computing (HPC) applications that run across multiple nodes in a cluster.

MPI allows processes to communicate with each other by sending and receiving messages explicitly.

---

## 1. MPI Basics

### What is MPI?

MPI is a library specification used for communication between processes in distributed memory environments. Each process has its own memory, and communication happens via messages.

### Key Concepts

#### Process

* An independent execution unit
* Each process has its own memory space
* Identified by a unique rank

#### Rank

* Each process is assigned a unique ID called a **rank**
* Rank 0 is usually the master process

#### Communicator

* A group of processes that can communicate with each other
* The default communicator is `MPI_COMM_WORLD`

#### Size

* Total number of processes in a communicator

---

### Basic MPI Program Structure

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    printf("Hello from process %d of %d\n", rank, size);

    MPI_Finalize();
    return 0;
}
```

### Execution

```bash
mpicc program.c -o program
mpirun -np 4 ./program
```

---

## 2. Point-to-Point Communication

Point-to-point communication involves data transfer between two specific processes.

---

### Blocking Communication

#### MPI_Send

Sends data from one process to another.

#### MPI_Recv

Receives data from another process.

### Example

```c
if (rank == 0) {
    int data = 100;
    MPI_Send(&data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD);
} else if (rank == 1) {
    int data;
    MPI_Recv(&data, 1, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
}
```

---

### Key Parameters

* Buffer: Data to send/receive
* Count: Number of elements
* Datatype: Type of data (MPI_INT, MPI_FLOAT, etc.)
* Destination/Source: Rank of the process
* Tag: Identifier for messages
* Communicator: Communication group

---

### Non-Blocking Communication

Allows overlap of computation and communication.

#### MPI_Isend

#### MPI_Irecv

#### MPI_Wait

### Example

```c
MPI_Request request;
MPI_Isend(&data, 1, MPI_INT, 1, 0, MPI_COMM_WORLD, &request);
MPI_Wait(&request, MPI_STATUS_IGNORE);
```

---

### Advantages

* Fine-grained control
* Flexible communication patterns

### Challenges

* Deadlocks
* Synchronization complexity

---

## 3. Collective Communication

Collective communication involves all processes in a communicator.

---

### Common Collective Operations

#### MPI_Bcast (Broadcast)

Send data from one process to all others.

```c
MPI_Bcast(&data, 1, MPI_INT, 0, MPI_COMM_WORLD);
```

---

#### MPI_Scatter

Distributes chunks of data from one process to all processes.

```c
MPI_Scatter(sendbuf, chunk, MPI_INT, recvbuf, chunk, MPI_INT, 0, MPI_COMM_WORLD);
```

---

#### MPI_Gather

Collects data from all processes to one process.

```c
MPI_Gather(sendbuf, chunk, MPI_INT, recvbuf, chunk, MPI_INT, 0, MPI_COMM_WORLD);
```

---

#### MPI_Reduce

Performs reduction operations (sum, max, etc.) across processes.

```c
MPI_Reduce(&local, &global, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);
```

---

#### MPI_Allreduce

Same as reduce, but result is available on all processes.

---

### Advantages

* Simplifies communication patterns
* Optimized by MPI implementations

### Use Cases

* Parallel reductions
* Data distribution
* Synchronization

---

## 4. Writing Distributed Programs

Distributed programs run across multiple nodes where each process has its own memory.

---

### Key Principles

#### Domain Decomposition

Divide the problem into smaller parts.

* **Data decomposition**: Split data among processes
* **Task decomposition**: Assign different tasks

---

#### Communication Strategy

Minimize communication overhead:

* Reduce message frequency
* Use collective operations when possible
* Overlap communication and computation

---

#### Load Balancing

Ensure all processes have equal work.

* Avoid idle processes
* Distribute data evenly

---

#### Synchronization

Processes must coordinate execution.

* Barriers (`MPI_Barrier`)
* Implicit synchronization in collectives

---

### Example: Parallel Sum

Each process computes a partial sum, then combines results.

```c
int local_sum = compute_local();
int global_sum;

MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);
```

---

### Common Design Patterns

* Master-Worker
* Domain Decomposition
* Pipeline
* MapReduce-like patterns

---

### Performance Considerations

* Communication latency
* Bandwidth
* Computation-to-communication ratio
* Network topology

---

### Debugging Tips

* Print rank-specific logs
* Use small number of processes first
* Check for deadlocks
* Validate communication patterns

---

## Summary

MPI is essential for distributed computing in HPC:

* Point-to-point communication for direct messaging
* Collective communication for group operations
* Distributed program design for scalability

Mastering MPI enables applications to scale across thousands of nodes efficiently.

