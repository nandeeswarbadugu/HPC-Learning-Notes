# Core Components of High Performance Computing (HPC)

High Performance Computing (HPC) systems are designed to solve complex computational problems by leveraging massive parallelism and optimized hardware. This document explains the fundamental components that make HPC systems powerful.

---

## 1. CPUs and Multi-Core Processing

### What is a CPU?

The Central Processing Unit (CPU) is the primary unit responsible for executing instructions. In HPC systems, CPUs are designed to handle multiple tasks efficiently.

### Multi-Core Architecture

Modern CPUs contain multiple cores within a single processor. Each core can execute instructions independently, enabling parallel execution.

### Key Concepts

* **Core**: Individual processing unit inside a CPU
* **Thread**: Smallest sequence of programmed instructions
* **Hyper-threading**: Allows a single core to handle multiple threads

### Importance in HPC

* Enables parallel execution of tasks
* Reduces computation time
* Supports shared memory parallelism (e.g., OpenMP)

---

## 2. GPUs and Accelerators

### What is a GPU?

A Graphics Processing Unit (GPU) is specialized hardware designed for parallel processing of large data blocks.

### Why GPUs in HPC?

GPUs have thousands of smaller cores optimized for throughput rather than latency.

### Key Features

* Massive parallelism
* High memory bandwidth
* Efficient for matrix operations

### Common Frameworks

* CUDA (NVIDIA)
* OpenCL
* HIP (AMD)

### Use Cases

* Machine learning and AI
* Scientific simulations
* Image and signal processing

---

## 3. Memory Hierarchy (Cache, RAM)

### Why Memory Hierarchy?

Accessing memory has different speeds. HPC systems use a hierarchy to optimize performance.

### Levels of Memory

1. **Registers** (fastest, smallest)
2. **Cache Memory** (L1, L2, L3)
3. **Main Memory (RAM)**
4. **Storage (SSD/HDD)** (slowest)

### Cache Types

* **L1 Cache**: Smallest and fastest (per core)
* **L2 Cache**: Larger but slightly slower
* **L3 Cache**: Shared across cores

### Importance in HPC

* Reduces latency
* Improves data locality
* Critical for performance optimization

---

## 4. High-Speed Interconnects (InfiniBand)

### What are Interconnects?

In HPC clusters, multiple nodes (computers) work together. Interconnects are the communication links between these nodes.

### Why High-Speed Interconnects?

In parallel computing, nodes must frequently exchange data. Slow communication becomes a bottleneck.

### InfiniBand Overview

InfiniBand is a high-performance networking technology widely used in HPC clusters.

### Key Features

* **Low Latency**: Extremely fast communication between nodes
* **High Bandwidth**: Supports large data transfers
* **RDMA (Remote Direct Memory Access)**: Allows direct memory access between nodes without CPU involvement

### How RDMA Works

RDMA allows one computer to directly read/write memory on another computer without involving the operating system or CPU, reducing overhead and latency.

### Communication Models

* **Message Passing (MPI)**
* **One-sided communication (RDMA)**

### InfiniBand vs Ethernet

| Feature      | InfiniBand | Ethernet |
| ------------ | ---------- | -------- |
| Latency      | Very Low   | Higher   |
| Bandwidth    | Very High  | Moderate |
| CPU Overhead | Low        | Higher   |
| Cost         | Expensive  | Cheaper  |

### Importance in HPC

* Enables efficient scaling to thousands of nodes
* Reduces communication bottlenecks
* Essential for distributed computing workloads

### Real-World Example

In large simulations (e.g., weather forecasting), thousands of nodes must exchange data at every timestep. InfiniBand ensures this communication happens quickly, preventing delays.

