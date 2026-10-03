# VM vs Container Performance Analysis

## 1. Project Overview

This project compares the performance of Virtual Machines (VMs) and Docker Containers using CPU, memory, disk I/O, network, application, startup-time, and scalability benchmarks.

---

## 2. Experimental Environment

| Component | Configuration |
|---|---|
| OS | Ubuntu 24.04 LTS |
| Virtualization | VMware Workstation |
| Container Platform | Docker |
| CPU | 2 vCPUs |
| Memory | 5.7 GB |
| Disk | 20 GB |
| Benchmark Tools | Sysbench, FIO, iperf3 |
| Application | FastAPI |
| Language | Python 3 |

---

## 3. Project Directory

    ~/vm-vs-container-performance

---



# Experiment 1 — CPU Performance

## Objective

Compare CPU performance using a prime-number calculation workload.

## Tool

`sysbench`

### VM

    mkdir -p ~/vm-vs-container-performance/results/raw/cpu/vm

    sysbench cpu --cpu-max-prime=20000 --threads=4 --time=30 run

### Save 10 Runs

    for i in {1..10}
    do
     sysbench cpu --cpu-max-prime=20000 --threads=4 --time=30 run > ~/vm-vs-container-performance/results/raw/cpu/vm/run$i.txt
    done

### Docker

    mkdir -p ~/vm-vs-container-performance/results/raw/cpu/container

    docker run --rm vm-container-benchmark sysbench cpu --cpu-max-prime=20000 --threads=4 --time=30 run

### Save 10 Runs

    for i in {1..10}
    do
     docker run --rm vm-container-benchmark sysbench cpu --cpu-max-prime=20000 --threads=4 --time=30 run > ~/vm-vs-container-performance/results/raw/cpu/container/run$i.txt
    done

### CPU Scalability

    for threads in 1 2 4 8
    do
     sysbench cpu --cpu-max-prime=20000 --threads=$threads --time=30 run
    done

### Metrics

- Events/sec
- Execution time
- CPU scalability

---

# Experiment 2 — Memory Performance

## Objective

Compare memory operation performance between VM and container.

## Tool

`sysbench`

### VM

    mkdir -p ~/vm-vs-container-performance/results/raw/memory/vm

    sysbench memory --memory-block-size=1M --memory-total-size=10G --threads=4 run

### Repeat for 10 Runs

    for i in {1..10}
    do
     sysbench memory --memory-block-size=1M --memory-total-size=10G --threads=4 run > ~/vm-vs-container-performance/results/raw/memory/vm/run$i.txt
    done

### Docker

    mkdir -p ~/vm-vs-container-performance/results/raw/memory/container

    for i in {1..10}
    do
     docker run --rm vm-container-benchmark sysbench memory --memory-block-size=1M --memory-total-size=10G --threads=4 run > ~/vm-vs-container-performance/results/raw/memory/container/run$i.txt
    done

### Resource Monitoring

    vmstat 1

    docker stats

### Metrics

- Memory operations/sec
- Latency
- CPU usage
- Memory usage

---

# Experiment 3 — Disk I/O Performance

## Objective

Compare sequential and random disk performance.

## Tool

`fio`

### Sequential Write

    fio --name=seq-write --filename=~/fio-test/testfile --size=2G --bs=1M --rw=write --direct=1 --iodepth=16 --runtime=30 --time_based

### Sequential Read

    fio --name=seq-read --filename=~/fio-test/testfile --size=2G --bs=1M --rw=read --direct=1 --iodepth=16 --runtime=30 --time_based

### Random Read

    fio --name=random-read --filename=~/fio-test/testfile --size=2G --bs=4k --rw=randread --direct=1 --iodepth=16 --runtime=30 --time_based

### Random Write

    fio --name=random-write --filename=~/fio-test/testfile --size=2G --bs=4k --rw=randwrite --direct=1 --iodepth=16 --runtime=30 --time_based

### Docker

    docker run --rm -v ~/fio-test:/fio-test vm-container-benchmark fio --name=seq-write --filename=/fio-test/testfile --size=2G --bs=1M --rw=write --direct=1 --iodepth=16 --runtime=30 --time_based

The same `fio` parameters were used for sequential read, random read, and random write workloads.

### Metrics

- Throughput
- IOPS
- Latency

---

# Experiment 4 — Network Performance

## Objective

Measure network throughput and retransmissions.

## Tool

`iperf3`

### Start Server

    iperf3 -s

### Find Server IP

    ip addr

### Client Test

    iperf3 -c <SERVER-IP> -t 30

### Parallel Streams

    iperf3 -c <SERVER-IP> -t 30 -P 4

### Save Result

    iperf3 -c <SERVER-IP> -t 30 > results/raw/network/iperf3.txt

### Metrics

- Network throughput
- Retransmissions
- Parallel-stream performance

---

# Experiment 5 — FastAPI Application Performance

## Objective

Compare application performance inside the VM and Docker container.

## FastAPI Application

    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/health")
    def health():
        return {"status": "healthy"}

    @app.get("/compute")
    def compute():
        total = 0
        for i in range(1_000_000):
            total += i * i
        return {"result": total}

    @app.get("/memory")
    def memory():
        data = [i for i in range(1_000_000)]
        return {"elements": len(data)}

### Install Dependencies

    python3 -m pip install fastapi uvicorn

### Run Application

    uvicorn main:app --host 0.0.0.0 --port 8000

### Test Application

    curl http://localhost:8000/health

### Docker Build

    docker build -t performance-api -f api/Dockerfile api

### Docker Run

    docker run --rm -p 8000:8000 performance-api

### Health Endpoint Test

    curl http://127.0.0.1:8000/health

### Load Testing

    ab -n 10000 -c 100 http://127.0.0.1:8000/health

    ab -n 1000 -c 10 http://127.0.0.1:8000/compute

### Optional wrk Test

    sudo apt install -y wrk

    wrk -t4 -c100 -d30s http://127.0.0.1:8000/health

### Metrics

- Requests/sec
- Response time
- Failed requests
- Connection time

---

# Experiment 6 — Startup Time

## Objective

Measure application startup time in VM and Docker.

### Docker Startup

    time docker run --rm -d --name startup-test -p 8000:8000 performance-api

### Check Application Readiness

    curl http://127.0.0.1:8000/health

### Stop Container

    docker stop startup-test

### Metrics

- Startup time
- Application-ready time

---

# Experiment 7 — Scalability

## Objective

Measure performance as workload and concurrency increase.

### CPU Scalability

    for threads in 1 2 4 8
    do
     sysbench cpu --cpu-max-prime=20000 --threads=$threads --time=30 run
    done

### API Scalability

    wrk -t1 -c10 -d30s http://127.0.0.1:8000/health

    wrk -t2 -c50 -d30s http://127.0.0.1:8000/health

    wrk -t4 -c100 -d30s http://127.0.0.1:8000/health

    wrk -t4 -c200 -d30s http://127.0.0.1:8000/health

### Metrics

- Throughput
- Latency
- CPU utilization
- Memory utilization
- Performance under increasing workload

---


  

# Conclusion

This project compares Virtual Machines and Docker Containers across:

- CPU performance
- Memory performance
- Disk I/O performance
- Network performance
- FastAPI application performance
- Startup time
- Scalability

The experiments use consistent workloads and benchmark tools to collect performance measurements for both environments.
