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

The following thread counts were tested:

1 thread
2 threads
4 threads
8 threads

Each benchmark run was performed for 30 seconds.

Docker CPU Benchmark

The Docker benchmark was executed using:

docker run --rm vm-container-benchmark sysbench cpu --cpu-max-prime=20000 --threads=<THREADS> --time=30 run
Metrics

The following metrics were collected from the sysbench output:

Events per second
Total execution time
Total number of events

Repeated measurements were collected to support later statistical analysis.

CPU Scalability

CPU scalability was examined by increasing the number of threads from 1 to 8.

Thread Count	VM	Docker
1	Tested	Tested
2	Tested	Tested
4	Tested	Tested
8	Tested	Tested
Screenshots

Execution and result screenshots for Experiment 1 are stored in:

screenshots/experiment-1-cpu/
Results

The raw CPU benchmark results were collected during the experiment.

Detailed statistical analysis, comparison tables, and graphs will be added after completion of the remaining experiments.

Conclusion

Experiment 1 establishes the CPU performance measurements required for the VM-versus-container comparison.

The final overall comparison will be based on measured results, statistical analysis, workload characteristics, and system configuration rather than assuming beforehand that one environment will be faster than the other.
