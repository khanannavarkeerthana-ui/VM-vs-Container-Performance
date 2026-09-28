# VM vs Container Performance Analysis

## Experiment 1: CPU Performance

### Objective

The objective of this experiment is to compare CPU performance between a Linux virtual machine and a Docker container under controlled workloads.

The CPU benchmark uses `sysbench` and measures performance at different thread counts.

### Experimental Environment

#### Virtual Machine

- Platform: VMware Workstation
- Operating System: Ubuntu 24.04.5 LTS
- CPU: 2 vCPUs
- Memory: 5.7 GiB
- Disk: 20 GB
- Kernel: Linux 7.0.0-34-generic

#### Docker

- Docker version: 29.1.3
- Benchmark image: `vm-container-benchmark`
- Base image: Ubuntu 24.04

### CPU Benchmark

The CPU workload was executed using:

```bash
sysbench cpu --cpu-max-prime=20000 --threads=<THREADS> --time=30 run
