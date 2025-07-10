# 🧮 CFS Scheduler Simulation

This project simulates the **Completely Fair Scheduler (CFS)** — a process scheduling algorithm used in the Linux kernel. It demonstrates how CFS manages **CPU-bound** and **I/O-bound** tasks while maintaining fairness using `vruntime` and `weight`.

---

## 📘 What is CFS?

The **Completely Fair Scheduler** ensures equal CPU distribution by scheduling the task with the **lowest virtual runtime (vruntime)**. Tasks with **higher weight** (priority) grow vruntime more slowly and hence get scheduled more frequently.

---

## 🧩 Focus Areas

### 🖥️ CPU-bound Tasks
- Consume CPU continuously.
- Executed for a **fixed time slice** (1ms).
- Lower vruntime leads to more frequent execution.

### 🧾 I/O-bound Tasks
- Sleep/wait frequently for I/O.
- Simulate I/O wait: **10ms**.
- Penalized in vruntime during I/O wait.
- Executed briefly (1ms) post I/O completion.

---

## ⚙️ Core Parameters

| Parameter   | Description |
|-------------|-------------|
| `weight`    | Priority of the task. Higher weight = higher priority. |
| `NICE_0_LOAD` | A constant used in weight calculation (`1024`). |
| `vruntime`  | `vruntime += (executed_time * NICE_0_LOAD) / weight` |

> **Note:** Lower vruntime = sooner the task is scheduled.

---

## 🧠 CFS Scheduling Logic

### 1. **Initialization**
- All tasks are inserted into the `runqueue` (min-heap based on vruntime).
- Each task is tagged as `CPU_BOUND` or `IO_BOUND`.

### 2. **Scheduling Loop**
- The task with the **smallest vruntime** is picked from the runqueue.
- Execution depends on task type:

#### 🔹 CPU-Bound Task:
- Run for **1ms**
- Update `vruntime`
- Decrease `cpu_burst_time`
- Reinsert if work remains

#### 🔹 I/O-Bound Task:
- Simulate I/O: `sleep(10ms)`
- Penalize `vruntime` during wait
- Execute for **1ms**
- Reinsert if work remains

### 3. **Termination**
- Ends when all tasks finish (`cpu_burst_time == 0`)

---

## 📊 Simulation Scenarios

### ✅ Scenario 1: All I/O-bound Tasks
- High-priority tasks get **more vruntime penalty** due to repeated I/O waits.
- They are scheduled **less frequently** despite higher priority.

### ✅ Scenario 2: All CPU-bound Tasks
- `Process 1` (highest priority, lowest initial vruntime) finishes first.
- Due to slower vruntime growth, it is scheduled more frequently.

> Observation: I/O-bound tasks suffer scheduling delays despite higher priority, while CPU-bound tasks with higher priority benefit more clearly from the CFS policy.



