# y86 Introduction to Pipelining


## 1. Motivation: The Problem with Sequential Execution
* **Inefficiency:** In a sequential processor, hardware utilization is poor.
    * *Fetch stage:* Only uses instruction memory.
    * *Decode stage:* Only uses register file.
    * *Execute stage:* Only uses ALU.
* **Idle Hardware:** While one component is working, the rest of the circuit sits idle waiting for signals to propagate through the entire loop.
* **Goal:** Use all hardware resources simultaneously to speed up total execution time.

## 2. The Concept: Pipelining
* **Analogy (Ponies & Pies):** * *Sequential:* One worker does every step (crust, fill, top) for one pie before starting the next. Slow.
    * *Pipelined:* Assembly line approach. Different workers handle different steps for different pies simultaneously.
* **Key Idea:** Break the instruction execution into discrete stages and overlap them.

## 3. Hardware Implementation
* **The 5 Stages:**
    1.  Fetch
    2.  Decode
    3.  Execute
    4.  Memory
    5.  Write Back
* **Pipeline Registers:** * New hardware added **between** each stage.
    * **Purpose:** hold (latch) the signals/data produced by the previous stage so the next stage can work on it independently.
* **PC Update:**
    * Removed as a distinct "stage" in the pipeline model.
    * Computing the next PC is treated as logic that feeds into the Fetch stage of the next cycle.

## 4. Visualizing Execution
* **Time:** Moves from left to right.
* **Instructions:** Stacked vertically (each row is a separate instruction).
* **Parallelism:** At any single point in time, up to 5 different instructions can be active, each in a different stage (e.g., Instruction 1 in Write Back, Instruction 2 in Memory, ..., Instruction 5 in Fetch).

## 5. Performance & Trade-offs
* **Throughput (Advantage):** * The total time to execute a *collection* of instructions decreases significantly.
    * Hardware is used much more efficiently.
* **Latency (Disadvantage):** * The time to complete a *single* instruction increases slightly due to the overhead of latching data into pipeline registers.
* **Pipeline Balancing:**
    * For maximum efficiency, all stages must take roughly the same amount of time.
    * *Bottleneck:* The pipeline runs at the speed of its slowest stage. If "Execute" takes 1000ns and "Decode" takes 1ns, the Decode stage spends 999ns waiting.

## 6. Challenges
* **Hazards:** Pipelining introduces logical problems (dependencies) when an instruction needs data that hasn't been computed or written back by a previous instruction yet.

# Drawbacks & Limitations of Pipelining

While pipelining increases **throughput** (instructions per second), it is not a perfect system. There are three main factors that limit its performance.

## 1. Nonuniform Partitioning (Imbalanced Stages)
Ideally, a computation is split into equal-length stages. In reality, hardware units (like ALUs or Memory) are difficult to subdivide perfectly.

*   **The Problem:** Some stages will inevitably be slower than others.
*   **The Constraint:** The clock cycle *must* be set to accommodate the **slowest stage**.
*   **The Consequence:**
    *   Faster stages sit **idle** (wasting time) waiting for the slow stage to finish.
    *   *Example:* If Stage A takes 50ps but Stage B takes 150ps, the clock must run at 150ps (+ overhead). Stage A is idle for 100ps every cycle.

## 2. Diminishing Returns (Pipeline Overhead)
Adding more stages (deep pipelining) generally increases speed, but there is a "tax" for every stage added.

*   **The Problem:** Pipeline registers are not instant. They have a fixed delay (e.g., 20 ps) to capture data.
*   **The Constraint:** Total Cycle Time = `Logic Time` + `Register Delay`.
*   **The Consequence:**
    *   As you split logic into smaller and smaller pieces, the **register delay** becomes a larger percentage of the total clock cycle.
    *   Eventually, the overhead outweighs the benefit of splitting the stages.
    *   **Latency increases:** The time to complete a single instruction gets longer due to the accumulated delay of passing through many registers.

## 3. Logical Dependencies (System with Feedback)
Pipelining assumes instructions are independent, but real programs (x86-64/Y86) have instructions that rely on each other.

*   **Data Dependencies:**
    *   Occurs when an instruction needs a value that the previous instruction hasn't finished calculating yet.
    *   *Example:*
        1. `irmovq $50, %rax` (Writes to %rax)
        2. `addq %rax, %rbx` (Reads %rax immediately)
    *   In a pipeline, Instruction 2 tries to read `%rax` before Instruction 1 has written to it.

*   **Control Dependencies:**
    *   Occurs due to conditional jumps (`jne`, `je`).
    *   The processor needs to load the next instruction immediately, but the "Jump" instruction is still in the pipeline and hasn't decided if it should jump yet.
    *   This requires "feedback paths" or stalling to ensure the correct instruction is executed.