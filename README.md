# VM vs Container Performance Analysis

## Project Overview

This project compares the performance of the same workloads running in:

- A Linux Virtual Machine running Ubuntu
- Docker containers running on the same Ubuntu VM

The experiments measure CPU, memory, disk I/O, network, application performance, startup time, and scalability.

The same workload parameters are used wherever possible so that the results can be compared under controlled conditions.

---

# Experimental Environment

## Virtual Machine

- Hypervisor: VMware Workstation
- Operating System: Ubuntu 24.04.5 LTS
- CPU: 2 vCPUs
- Memory: 5.7 GiB
- Disk: 20 GB
- Kernel: Linux 7.0.0-34-generic

## Docker

- Docker version: 29.1.3
- Benchmark image: `vm-container-benchmark`
- Base image: Ubuntu 24.04

## Project Directory

```text
~/vm-vs-container-performance
