# Containers in HPC

Containers are a key technology in High Performance Computing (HPC) that allow applications to run in isolated, portable, and reproducible environments. They solve dependency issues and ensure that software behaves the same across different systems.

This document covers:

* Docker basics
* Singularity / Apptainer
* Running containers on HPC clusters

---

## 1. Why Containers in HPC?

HPC environments often involve complex software stacks and dependencies. Containers help by packaging everything needed to run an application.

### Benefits

* Reproducibility (same environment everywhere)
* Portability across systems
* Isolation from host system
* Simplified dependency management

### Challenges in HPC

* Security restrictions (no root access)
* Performance considerations
* Integration with schedulers (SLURM)

---

## 2. Docker Basics

Docker is a widely used container platform for building and running containers.

---

### Key Concepts

#### Image

A lightweight, read-only template that contains:

* OS libraries
* Dependencies
* Application code

#### Container

A running instance of an image.

#### Dockerfile

A script used to build Docker images.

---

### Basic Commands

#### Build an Image

```bash
docker build -t my_image .
```

#### Run a Container

```bash
docker run my_image
```

#### Run with Interactive Shell

```bash
docker run -it my_image /bin/bash
```

#### List Containers

```bash
docker ps
```

---

### Example Dockerfile

```dockerfile
FROM ubuntu:20.04

RUN apt-get update && apt-get install -y python3

COPY app.py /app.py

CMD ["python3", "/app.py"]
```

---

### Limitations in HPC

* Requires root privileges
* Not suitable for multi-user shared clusters
* Security risks

Because of these limitations, Docker is typically not used directly on HPC clusters.

---

## 3. Singularity / Apptainer

Singularity (now Apptainer) is a container platform designed specifically for HPC environments.

---

### Why Singularity?

* Runs without root privileges
* Integrates with HPC schedulers
* Maintains high performance
* Uses host system resources directly

---

### Key Features

* User-level execution
* Native access to GPUs and MPI
* File system integration

---

### Basic Commands

#### Pull Image from Docker Hub

```bash
singularity pull docker://ubuntu:20.04
```

---

#### Run a Container

```bash
singularity run ubuntu_20.04.sif
```

---

#### Execute a Command

```bash
singularity exec ubuntu_20.04.sif ls
```

---

#### Interactive Shell

```bash
singularity shell ubuntu_20.04.sif
```

---

### Building Images

```bash
singularity build my_image.sif recipe.def
```

---

### Example Definition File

```def
Bootstrap: docker
From: ubuntu:20.04

%post
    apt-get update
    apt-get install -y python3

%runscript
    python3 --version
```

---

## 4. Running Containers on HPC Clusters

Containers are typically run inside jobs managed by a scheduler like SLURM.

---

### Example SLURM Job Script with Singularity

```bash
#!/bin/bash
#SBATCH --job-name=container_job
#SBATCH --ntasks=1
#SBATCH --time=00:10:00
#SBATCH --partition=compute

module load singularity

singularity exec my_image.sif ./my_program
```

---

### Running MPI Jobs in Containers

```bash
#!/bin/bash
#SBATCH --nodes=2
#SBATCH --ntasks=8

module load singularity

srun singularity exec my_image.sif ./mpi_program
```

---

### GPU Jobs with Containers

```bash
#SBATCH --gres=gpu:1

singularity exec --nv my_image.sif ./gpu_program
```

* `--nv` enables NVIDIA GPU support

---

### Binding Directories

Access host files inside container:

```bash
singularity exec --bind /data:/mnt my_image.sif ls /mnt
```

---

### Environment Variables

```bash
export VAR=value
singularity exec my_image.sif env
```

---

## 5. MPI and Containers

MPI applications require special handling.

---

### Two Approaches

#### Host MPI

* Use MPI installed on host
* Container uses host MPI libraries

#### Container MPI

* MPI installed inside container
* Must be compatible with host network

---

### Best Practice

Use **host MPI with containerized application** for best performance.

---

## 6. Performance Considerations

* Containers have near-native performance
* Avoid unnecessary layers in images
* Use optimized base images
* Minimize I/O overhead

---

## 7. Best Practices

* Use Singularity/Apptainer for HPC
* Keep images lightweight
* Version your images
* Test locally before cluster execution
* Use SLURM for job management

---

## 8. Common Use Cases

* Machine learning workflows
* Scientific simulations
* Reproducible research
* Software portability

---

## Summary

Containers in HPC enable:

* Reproducible environments
* Portability across systems
* Efficient execution on clusters

Docker is useful for building images, while Singularity/Apptainer is preferred for running containers in HPC environments.

