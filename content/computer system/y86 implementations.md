# Y86 Processor Implementation Notes

These notes summarize the implementation of the Y86 processor pipeline, broken down by stage. This material is based on lectures by Margo Seltzer covering the Fetch, Decode, Execute, Memory, Write Back, and PC Update stages.

---

## 1. Fetch Stage
**Goal:** Read the instruction from memory and prepare for execution.

* **Primary Inputs:**
    * **Program Counter (PC):** Provides the memory address of the instruction to be fetched.
    * **Memory:** The storage from which instruction bytes are read.
* **Key Actions:**
    * **Read Instruction:** The PC sends the address to memory.
    * **Split Instruction:** The instruction bytes are parsed into specific fields rather than a single large register.
        * **`icode` (Opcode):** Determines the instruction type.
        * **`ifun` (Function):** Specifies the operation variation (e.g., add vs. sub).
        * **`rA` & `rB`:** Register identifiers (if used by the instruction).
        * **`valC`:** A constant value (if used, e.g., for immediate values or displacements).
    * **Logic:**
        * **Instruction Validity:** Checks if the instruction is valid.
        * **Instruction Length:** The `icode` determines how many bytes were read (1, 2, 9, or 10 bytes).
    * **Compute Next PC (`valP`):**
        * Calculates `valP = current PC + instruction length`.
        * This is the "potential" next address, used if no jump or call occurs.



---

## 2. Decode Stage
**Goal:** Read necessary values from the Register File.

* **Inputs:**
    * **`rA` & `rB`:** From the Fetch stage.
    * **`icode`:** Crucial for determining implicit registers.
* **Key Components:**
    * **Register File:** The internal storage bank for registers.
    * **Selection Logic (`srcA` & `srcB`):** Determines which register IDs to send to the Register File.
* **Logic Flow:**
    * **Standard Instructions:** `rA` and `rB` are used directly to read values `valA` and `valB` (e.g., `addq`, `rrmovq`).
    * **Stack Instructions:** For `push`, `pop`, `call`, and `ret`, the logic detects the `icode` and automatically selects `%rsp` (Stack Pointer) as the source, even though it isn't explicitly named in `rA` or `rB`.
* **Outputs:**
    * **`valA`:** Value from the first source register.
    * **`valB`:** Value from the second source register.



---

## 3. Execute Stage
**Goal:** Perform arithmetic/logic operations and handle Condition Codes (CC).

* **Inputs:**
    * `valA`, `valB`, `valC`.
    * `icode`, `ifun`.
* **Key Components:**
    * **ALU (Arithmetic Logic Unit):** Performs the calculation.
    * **Condition Codes (CC):** Flags (Zero, Sign, Overflow) storing the status of the last arithmetic operation.
    * **ALU Input Logic (`ALU A` & `ALU B`):** Selects actual inputs for the ALU.
* **Operations:**
    * **Standard Arithmetic:** Uses `valA` and `valB` (controlled by `ifun`).
    * **Address Calculation:** Treated as `valB + valC` (Base + Displacement). The ALU adds them to produce `valE` (Effective Address).
    * **Stack Adjustment:** `push`/`pop`/`call`/`ret` use the ALU to add or subtract 8 from the stack pointer.
    * **Move Operations:** `rrmovq` (Register-Register Move) uses the ALU to add 0 to the value, passing it through to `valE` to simplify wiring.
* **Logic:**
    * **Set CC:** Controlled by `icode`. Updates Condition Codes *only* for arithmetic instructions, not for address calculations or stack adjustments.
    * **Cond (Conditional):** Uses `ifun` and current CC flags to determine if a conditional jump or move should proceed (computes `Cnd` signal).



---

## 4. Memory Stage
**Goal:** Read from or write to Data Memory.

* **Inputs:**
    * `valE` (Address calculated by ALU).
    * `valA` (Data to write).
    * `valP` (Return address for `call`).
    * `valB` (Original stack pointer for `pop`/`ret`).
* **Key Operations:**
    * **Address Selection:**
        * **Standard:** Uses `valE` as the address.
        * **Pop/Ret:** Uses `valB` (original stack pointer) as the address because reading must happen *before* the stack pointer is incremented.
    * **Data Write Selection:**
        * **Standard:** Writes `valA` to memory.
        * **Call:** Writes `valP` (PC of next instruction) to memory.
    * **Data Read:**
        * Reads from memory into **`valM`**.
    * **Return Address:** For `ret`, the value read (`valM`) is treated as the next instruction address.



---

## 5. Write Back Stage
**Goal:** Write results back to the Register File.

* **Inputs:**
    * `valE` (ALU result).
    * `valM` (Memory read result).
* **Logic:**
    * **Destination Selection (`destE`, `destM`):**
        * `valE` is usually written to `rB`.
        * `valM` is usually written to `rA`.
    * **Special Cases:**
        * **Conditional Moves:** If the `Cnd` signal (from Execute) is false, the write to the register is disabled.
        * **Stack Ops:** Ensures `%rsp` is updated correctly.

---

## 6. PC Update Stage
**Goal:** Select the PC for the next cycle.

* **Inputs:**
    * `valP` (Next sequential PC).
    * `valC` (Jump target / Call destination).
    * `valM` (Return address from memory).
    * `Cnd` (Condition signal from Execute).
* **Logic:**
    * **Select Next PC:**
        * **Sequential:** Default is `valP`.
        * **Jump/Call:** If taking a branch (`Cnd` is true for conditional jumps), select `valC`.
        * **Return:** Select `valM`.

    ---
# **some implementations**

![[image-3.png]]


![[image-4.png]]

# push

| Stage         | Operation                                      |
|:---           |:---                                            |
| **Fetch** | `icode:ifun = M1[PC]`<br>`rA:rB = M1[PC+1]`<br>`valP = PC + 2` |
| **Decode** | `valA = R[rA]`<br>`valB = R[%rsp]`             |
| **Execute** | `valE = valB + (-8)`                           |
| **Memory** | `M[valE] = valA`                               |
| **Write Back**| `R[%rsp] = valE`                               |
| **PC Update** | `PC = valP`                                    |                                   |


# rrmovq
| Stage         | Operation                                      |
|:---           |:---                                            |
| **Fetch** | `icode:ifun = M1[PC]`<br>`rA:rB = M1[PC+1]`<br>`valP = PC + 2` |
| **Decode** | `valA = R[rA]`                                 |
| **Execute** | `valE = 0 + valA`                              |
| **Memory** | *(No operation)* |
| **Write Back**| `R[rB] = valE`                                 |
| **PC Update** | `PC = valP`                                    |



![[y86ImplementationDiagram.png]]