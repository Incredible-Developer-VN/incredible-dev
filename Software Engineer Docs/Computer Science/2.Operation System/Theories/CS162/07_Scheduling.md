# Scheduling

## 🧠 Scheduling Concepts
**Goal**: Decide which thread/process gets the CPU next when multiple are ready.

---

## ⚙️ Contexts Where Scheduling Happens
- Thread **yields** the CPU voluntarily  
- Thread **blocks** (e.g., for I/O or lock)  
- Thread **exits**  
- **Preemption** (due to timer interrupt or higher-priority thread)

---

## 🎯 Scheduling Objectives (Evaluation Metrics)

| Metric          | Description                                |
| --------------- | ------------------------------------------ |
| CPU Utilization | Keep CPU busy                              |
| Throughput      | Number of jobs completed per time unit     |
| Turnaround Time | Time from submission to completion         |
| Waiting Time    | Time job spends in the ready queue         |
| Response Time   | Time from submission to first output       |
| Fairness        | Each job gets a fair share of CPU          |
| Predictability  | Avoid starvation and unpredictable latency |

---

## 📜 Classic Scheduling Policies

### 1. First-Come, First-Served (FCFS)
- Non-preemptive  
- Jobs run in arrival order  
- ❌ Convoy effect: long jobs block short ones

### 2. Shortest Job First (SJF) / Shortest Remaining Time First (SRTF)
- SJF: Non-preemptive; SRTF: Preemptive  
- Optimal average waiting time (if durations are known)  
- ❌ Starvation of long jobs  
- ❌ Hard to estimate job length

### 3. Round Robin (RR)
- Preemptive, time-sliced (quantum)  
- Fair: each thread gets CPU periodically  
- ✔️ Good for interactive systems  
- ❌ Overhead if time slice is too small

### 4. Priority Scheduling
- Scheduler always picks thread with highest priority  
- Can be preemptive or non-preemptive  
- ❌ Starvation of low-priority threads  
- ➕ Fix: Aging (gradually increase waiting threads' priority)

---

## 🧪 Time Slice (Quantum) Tuning in RR
- Too small → too many context switches  
- Too large → poor response time (acts like FCFS)

---

## 🔁 Preemptive vs. Non-Preemptive

| Type           | Can interrupt running thread? | Suitable for           |
| -------------- | ----------------------------- | ---------------------- |
| Preemptive     | ✅ Yes                         | Interactive, real-time |
| Non-Preemptive | ❌ No                          | Simple batch jobs      |

---

## 🧩 Policy Comparison

| Policy      | Preemptive? | Fairness | Response Time   | Starvation Risk | Implementation |
| ----------- | ----------- | -------- | --------------- | --------------- | -------------- |
| FCFS        | ❌           | ❌        | Poor            | ❌               | Easy           |
| SJF/SRTF    | ❌ / ✔️       | ❌        | Best (if known) | ✔️               | Complex        |
| Round Robin | ✔️           | ✔️        | Good            | ❌               | Easy           |
| Priority    | ✔️ / ❌       | ❌        | Varies          | ✔️               | Moderate       |

---

## 🧠 Key Takeaways
- No single best policy — depends on system goals  
- SJF minimizes waiting time but is impractical  
- RR balances fairness and responsiveness  
- Priority needs care to avoid starvation  
- Always tune time slice carefully in RR

---

## 🧠 What is Real-Time Scheduling?
**Real-Time System**: A system where correctness depends not only on the result, but also on *when* it is produced.

- Examples:
  - Embedded systems (cars, robots)
  - Medical devices
  - Video/audio systems

---

## 🎯 Hard vs. Soft Real-Time

| Type               | Description                               |
| ------------------ | ----------------------------------------- |
| **Hard Real-Time** | Missing a deadline = system failure       |
| **Soft Real-Time** | Occasional deadline misses are acceptable |

---

## 📋 Scheduling Goals for Real-Time Systems
- Predictable response times
- Meet deadlines
- Avoid starvation
- Handle priority inversion

---

## 🔢 Real-Time Scheduling Algorithms

### 1. Rate Monotonic Scheduling (RMS)
- Static priority: shorter period → higher priority
- Optimal among fixed-priority schedulers (if utilization < ~69%)

### 2. Earliest Deadline First (EDF)
- Dynamic priority: earliest deadline runs first
- If utilization ≤ 100%, all deadlines are met

---

## 🧱 Forward Progress & Starvation

### Starvation
- A thread never gets to run
- Causes:
  - Priority scheduling without aging
  - Preemption from higher-priority tasks

### Forward Progress
- Ensures every thread gets CPU eventually

#### Solutions:
- **Aging**: Increase waiting threads' priority over time
- **Fair schedulers**: Example: Linux CFS (Completely Fair Scheduler)

---

## 🚧 Priority Inversion

### Scenario:
- Low-priority thread holds lock
- High-priority thread waits for lock
- Medium-priority thread preempts low → high is blocked

### Fix: Priority Inheritance
- Temporarily boost priority of lock-holding thread

---

## 🔁 Summary Table

| Concept              | Purpose                                | Problem Solved             |
| -------------------- | -------------------------------------- | -------------------------- |
| Rate Monotonic (RMS) | Hard real-time tasks w/ fixed priority | Periodic scheduling        |
| EDF                  | Dynamic deadlines                      | Maximize schedulability    |
| Aging                | Fairness                               | Avoid starvation           |
| CFS (Linux)          | Proportional CPU share                 | Forward progress           |
| Priority Inheritance | Temporary priority boost               | Prevent priority inversion |

---

## 🧠 What is Deadlock?

**Deadlock** occurs when a group of processes are blocked because each is waiting for a resource held by another, forming a cycle of dependencies.

### Classic Example:
- Thread A holds Lock1, waiting for Lock2
- Thread B holds Lock2, waiting for Lock1
→ Both are blocked forever → deadlock

---

## 🪤 Coffman’s Conditions for Deadlock

Deadlock can occur **only if all 4 conditions hold simultaneously**:

| Condition            | Description                                         |
| -------------------- | --------------------------------------------------- |
| **Mutual Exclusion** | Resource can be held by only one process at a time  |
| **Hold and Wait**    | Process holds one resource while waiting for others |
| **No Preemption**    | Resource cannot be forcibly taken away              |
| **Circular Wait**    | Circular chain of processes waiting on each other   |

👉 Breaking **any one** of these conditions can prevent deadlock.

---

## 🔍 Deadlock Detection

### 1. Resource Allocation Graph (RAG)
- Nodes: processes and resources
- Edges:
  - P → R (request)
  - R → P (assignment)
- A **cycle in the graph** implies **potential deadlock**

---

## 🧯 Strategies to Handle Deadlock

### 1. **Ignore Deadlock**
- Do nothing (used in Linux and many real systems)
- Accept occasional crash or manual recovery

### 2. **Prevention** – Break one of Coffman’s conditions

| Condition        | How to prevent it                       |
| ---------------- | --------------------------------------- |
| Mutual Exclusion | Use shareable resources if possible     |
| Hold and Wait    | Require all resources at once           |
| No Preemption    | Allow OS to forcibly take resources     |
| Circular Wait    | Impose a global resource order ✅ common |

---

### 3. **Avoidance** – Use knowledge of future requests

#### Banker’s Algorithm:
- Only grant resources if it keeps system in a **safe state**
- Requires:
  - Knowing max demand in advance
  - Keeping track of resource state

→ Impractical in real systems due to high overhead

---

### 4. **Detection & Recovery**
- Let deadlock happen, then detect & fix

Steps:
- Detect cycles in RAG
- Kill or rollback processes
- Preempt resources

→ High cost, risk of data loss

---

## ⚖️ Strategy Comparison

| Strategy             | Pros                | Cons                          |
| -------------------- | ------------------- | ----------------------------- |
| Ignore               | Simple              | Risk of crash                 |
| Prevention           | Safe                | May reduce performance        |
| Avoidance (Banker)   | Effective in theory | Impractical in real systems   |
| Detection & Recovery | Flexible            | Complex, expensive, data loss |

---

## ⚠️ Priority Inversion

- Low-priority thread holds resource needed by high-priority thread
→ High-priority thread gets blocked

### Fix: **Priority Inheritance**
- Temporarily raise the priority of the low-priority thread

---

## 🎯 Summary

- Deadlock arises when **all 4 Coffman conditions hold**
- Common strategies:
  - **Prevent** (e.g., global ordering)
  - **Avoid** (Banker)
  - **Detect & Recover**
  - **Ignore** (simple systems)
- Real systems often balance between safety and simplicity