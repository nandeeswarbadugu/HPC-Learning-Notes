# Job Scheduling with SLURM

SLURM (Simple Linux Utility for Resource Management) is a widely used workload manager for High Performance Computing (HPC) clusters. It is responsible for allocating resources, scheduling jobs, and managing execution across compute nodes.

---

## 1. What is SLURM?

SLURM is an open-source job scheduler used to manage compute resources in a cluster.

### Responsibilities

* Allocates compute resources (CPU, memory, nodes)
* Queues jobs
* Schedules job execution
* Monitors job status

---

## 2. Writing Job Scripts

A SLURM job script is a Bash script that contains directives to request resources and run applications.

### Basic Structure

```bash
#!/bin/bash
#SBATCH --job-name=my_job
#SBATCH --output=output.txt
#SBATCH --error=error.txt
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --time=00:10:00
#SBATCH --partition=compute

# Load modules
module load gcc

# Run program
./my_program
```

---

### Common Directives

| Directive       | Description                     |
| --------------- | ------------------------------- |
| --job-name      | Name of the job                 |
| --output        | Output file                     |
| --error         | Error file                      |
| --ntasks        | Number of tasks (MPI processes) |
| --cpus-per-task | Threads per task                |
| --nodes         | Number of nodes                 |
| --time          | Job time limit                  |
| --partition     | Queue/partition                 |

---

## 3. SLURM Commands

---

### sbatch (Submit Job)

Submits a batch job script to the scheduler.

```bash
sbatch job.sh
```

---

### srun (Run Job)

Runs a job or task interactively or within a job allocation.

```bash
srun ./my_program
```

#### Example with MPI

```bash
srun -n 4 ./mpi_program
```

---

### squeue (View Jobs)

Displays the status of jobs in the queue.

```bash
squeue
```

---

### scancel (Cancel Job)

```bash
scancel <job_id>
```

---

### sinfo (Cluster Info)

Displays information about nodes and partitions.

```bash
sinfo
```

---

## 4. Resource Allocation

SLURM allows users to request specific resources for their jobs.

---

### Key Resources

#### Nodes

Physical machines in the cluster.

```bash
#SBATCH --nodes=2
```

---

#### Tasks

Usually corresponds to MPI processes.

```bash
#SBATCH --ntasks=8
```

---

#### CPUs per Task

Threads per process.

```bash
#SBATCH --cpus-per-task=4
```

---

#### Memory

```bash
#SBATCH --mem=8G
```

---

#### GPUs

```bash
#SBATCH --gres=gpu:2
```

---

### Example Allocation

```bash
#SBATCH --nodes=2
#SBATCH --ntasks=8
#SBATCH --cpus-per-task=2
#SBATCH --mem=16G
```

This requests:

* 2 nodes
* 8 total tasks
* 2 CPUs per task
* 16GB memory

---

## 5. Running Multi-node Jobs

Multi-node jobs are required for distributed applications (e.g., MPI programs).

---

### Example MPI Job Script

```bash
#!/bin/bash
#SBATCH --job-name=mpi_job
#SBATCH --nodes=2
#SBATCH --ntasks=8
#SBATCH --time=00:10:00
#SBATCH --partition=compute

module load mpi

srun -n 8 ./mpi_program
```

---

### How It Works

* SLURM allocates multiple nodes
* Each node runs multiple processes
* Processes communicate using MPI

---

### Task Distribution

Example:

```
2 nodes, 8 tasks
→ 4 tasks per node
```

---

### Hybrid Jobs (MPI + OpenMP)

```bash
#SBATCH --nodes=2
#SBATCH --ntasks=4
#SBATCH --cpus-per-task=8

export OMP_NUM_THREADS=8

srun ./hybrid_program
```

* 4 MPI processes
* Each process uses 8 threads

---

## 6. Job States

| State | Meaning    |
| ----- | ---------- |
| PD    | Pending    |
| R     | Running    |
| CG    | Completing |
| CD    | Completed  |
| F     | Failed     |

---

## 7. Best Practices

* Request only required resources
* Use appropriate partitions
* Monitor jobs using squeue
* Test with small jobs first
* Use job arrays for batch processing

---

## 8. Debugging Jobs

* Check output and error files
* Use `scontrol show job <job_id>`
* Run jobs interactively for debugging

---

## Summary

SLURM is essential for managing HPC workloads:

* Job scripts define resource needs
* sbatch submits jobs
* srun executes tasks
* squeue monitors jobs

Understanding SLURM enables efficient use of HPC clusters and scaling applications across nodes.
