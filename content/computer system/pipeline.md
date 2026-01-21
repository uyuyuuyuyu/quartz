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