# VM vs Container Performance Analysis

## Project Overview

This project presents a comparative performance analysis of workloads executed in a Linux Virtual Machine and Docker containers.

The experiments evaluate:

- CPU Performance
- Memory Performance
- Disk I/O Performance
- Network Performance
- FastAPI Application Performance
- Startup Time
- Scalability

The same workload configurations are used for the VM and Docker wherever applicable to provide a controlled comparison.

---

# Experimental Environment

## Virtual Machine

| Parameter | Configuration |
|---|---|
| Hypervisor | VMware Workstation |
| Operating System | Ubuntu 24.04.5 LTS |
| CPU | 2 vCPUs |
| Memory | 5.7 GiB |
| Disk | 20 GB |
| Kernel | Linux 7.0.0-34-generic |

## Docker

| Parameter | Configuration |
|---|---|
| Docker Version | 29.1.3 |
| Benchmark Image | `vm-container-benchmark` |
| Base Image | Ubuntu 24.04 |

## Project Directory

```text
~/vm-vs-container-performance
