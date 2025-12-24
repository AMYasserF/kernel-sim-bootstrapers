# OS Scheduler & Memory Manager Simulation

A simulation of an Operating System kernel’s **process scheduling** and **memory management** subsystems.  
The project models the full lifecycle of processes—from creation to termination—while handling CPU scheduling and dynamic memory allocation using **Unix IPC mechanisms**.

---

## 🚀 Features

### 📅 CPU Scheduling Algorithms
The scheduler supports three algorithms:

1. **Round Robin (RR)**  
   - Preemptive scheduling with configurable time quantum  
   - Queue-based ready list  

2. **Highest Priority First (HPF)**  
   - Non-preemptive scheduling  
   - Priority-based selection using a Min-Heap  

3. **Shortest Remaining Time Next (SRTN)**  
   - Preemptive scheduling  
   - Selects the process with the least remaining execution time using a Min-Heap  

---

### 💾 Memory Management
- **Buddy System Allocation** using a binary tree structure  
- Dynamic block splitting and coalescing to reduce fragmentation  
- **Total simulated memory**: 1024 bytes  
- Real-time allocation and deallocation logging to `memory.log`  

---

### 🔧 Inter-Process Communication (IPC)
System components communicate using **System V IPC**:

- **Message Queues** – Transfer process control blocks (PCBs) from generator to scheduler  
- **Shared Memory** – Share remaining execution time between scheduler and process  
- **Semaphores** – Synchronize execution and preemption  
- **Signals** – `SIGUSR1`, `SIGUSR2`, `SIGINT`, `SIGSTOP`, `SIGCONT` for state transitions  

---

## 📂 Project Architecture

- **`process_generator.c`**  
  Initializes the system clock and scheduler, reads process input, allocates memory via the Buddy System, and sends processes to the scheduler.

- **`scheduler.c`**  
  Core scheduling logic. Manages ready queues/heaps, handles context switching, and controls process execution.

- **`memory_manager.c`**  
  Implements Buddy System allocation, including block splitting, traversal, and merging.

- **`clk.c`**  
  Acts as the system clock, emitting a signal every second to synchronize components.

- **`process.c`**  
  Simulated process that decrements its remaining time and notifies the scheduler upon completion.

---

## 🛠️ Build & Run

### Prerequisites
- GCC Compiler  
- Make  
- Linux environment (required for System V IPC headers)

---

### Compilation

Navigate to the `src` directory and build the project:

```bash
cd src
make
```

Executables will be generated in `src/bin/`:
- `os-sim` (Process Generator)
- `scheduler`
- `process.out`

---

## ▶️ Usage

Run the simulation using `os-sim`:

```bash
./bin/os-sim -s <algorithm> -f <process_file> [-q <quantum>]
```

### Examples

```bash
# Round Robin with quantum = 2
./bin/os-sim -s rr -f processes.txt -q 2

# Highest Priority First
./bin/os-sim -s hpf -f processes.txt

# Shortest Remaining Time Next
./bin/os-sim -s srtn -f processes.txt
```

### Makefile Shortcuts

```bash
make run-rr
make run-hpf
make run-srtn
```

---

## 📝 Input File Format

Process definition file (e.g., `processes.txt`):

```text
# id arrival runtime priority mem_size
1 1 5 2 64
2 3 4 1 128
3 5 2 3 32
```

Lines starting with `#` are ignored.

---

## 📊 Outputs & Logs

- **`scheduler.log`**  
  Process state transitions (STARTED, STOPPED, RESUMED, FINISHED) and timing metrics

- **`scheduler.perf`**  
  - CPU Utilization  
  - Average Waiting Time  
  - Average Weighted Turnaround Time (WTA)  
  - Standard Deviation of WTA  

- **`memory.log`**  
  Memory allocation and deallocation events with timestamps and address ranges

---

## 🧹 Cleanup

Remove compiled binaries and object files:

```bash
make clean
```
