# Y86 Architecture: Call and Return Instructions

## 1. The Core Problem & Solution
* **Problem:** Programs need to invoke functions from various locations and return to the correct "next" instruction upon completion .
* **Solution:** Use the **Stack** to store the return address temporarily.

## 2. The `call` Instruction
* **Encoding:** Opcode `8` followed by an 8-byte target address .
* **Mechanism (The "Push + Jump"):**
    1. **Push Return Address:** Decrements the stack pointer (`%rsp`) and stores the address of the instruction *immediately following* the call onto the stack .
    2. **Update PC:** Sets the Program Counter (PC) to the target address of the function .
* **Conceptual View:** `call` acts as a `push` of the PC followed by a jump .

![[image.png]]

## 3. The `ret` (Return) Instruction
* **Encoding:** Opcode `90` .
* **Mechanism (The "Pop + Jump"):**
    1. **Read Address:** Reads the return address from the memory location currently pointed to by `%rsp` .
    2. **Update PC:** Moves that address back into the Program Counter (PC) to resume execution in the caller .
    3. **Cleanup:** Increments the stack pointer (`%rsp`) to effectively "pop" the address off the stack .
![[image-1.png]]
## 4. Key Takeaways
* **Stack State:** The stack must be in the same state when `ret` is called as it was after the `call` instruction finished for the program to return correctly .
* **Analogy:** `call` = Push + Jump; `ret` = Jump to address on stack .
* **Security Relevance:** Mastering how these instructions manipulate the stack is fundamental to understanding and executing **buffer overflow attacks** .


# Note: Procedure (y86 Architecture)

**Source:** (https://www.youtube.com/watch?v=gGwb8iQ2jAs)

## Definition
A **procedure** (often used interchangeably with "function") is a higher-level construct that allows a program to execute a specific block of code and then return control to the location where it was called.

## Key Mechanisms in y86
* **Communication:** Procedures facilitate interaction between the **Caller** (the function invoking the procedure) and the **Callee** (the procedure being invoked).
* **Parameter Passing:**
    * **Registers:** The first 6 arguments are passed via specific registers (`%rdi`, `%rsi`, `%rdx`, `%rcx`, `%r8`, `%r9`) .
    * **Stack:** Any arguments beyond the sixth, or arguments too large for registers (like structs), are passed on the stack .
* **Return Values:** The result of a procedure is always returned in the **`%rax`** register .
* **State Management:** Procedures adhere to **Calling Conventions** to manage register data:
    * **Callee-Saved:** The procedure must preserve specific registers (`%rbx`, `%rbp`, `%r12`-`%r14`) if it intends to use them (restore before returning).
    * **Caller-Saved:** The procedure is free to overwrite "scratch" registers (`%rax`, `%r10`, `%r11`, and argument registers) if the caller wants to use them it must save before the call.


# y86 Stack Frames

**Source:** [y86 Stack Frames Lecture](https://www.youtube.com/watch?v=iH9jotABTbU)

## 1. Overview
A **Stack Frame** is a structured collection of data on the stack dedicated to a single procedure call. It allows the architecture to support:
* **Recursion:** Each instance of a function gets its own storage.
* **Parameter Passing:** For arguments that do not fit in registers.
* **Local Variables:** Storage for variables local to the function.
* **Control Flow:** Storing return addresses.

**Stack Direction:** The stack grows from **high addresses** to **low addresses**.

---

## 2. Stack Frame Contents
A typical stack frame usually contains the following (ordered from high address to low address):
1.  **Caller's Return Address:** Pushed automatically by the `call` instruction.
2.  **Saved Base Pointer (Optional):** If using a frame pointer (`%rbp`).
3.  **Local Variables:** Space allocated by the compiler for the function's internal use.
4.  **Spill Space:** (Implicit) For saving registers if needed.

---

## 3. Management Method A: No Base Pointer
In this method, the stack frame is managed solely using the **Stack Pointer (`%rsp`)**.

### **Preamble (Setup)**
The compiler generates code to allocate space immediately upon entering the function.
* **Action:** Subtract the required size (in bytes) from `%rsp`.
    * *Example:* `subq $32, %rsp`
* **Accessing Data:**
    * **Locals:** Accessed via **positive offsets** from `%rsp`.
    * **Arguments:** Accessed via larger **positive offsets** from `%rsp` (past the locals and return address).

### **Postamble (Teardown)**
* **Action:** Add the size back to `%rsp` to deallocate the space.
    * *Example:* `addq $32, %rsp`
* **Return:** The `ret` instruction is executed immediately after to pop the return address.

---

## 4. Management Method B: With Base Pointer (`%rbp`)
This method uses `%rbp` as a **Frame Pointer** to provide a stable reference point for the stack frame, which is useful if `%rsp` moves (e.g., during push/pop operations).

### **Preamble (Setup)**
1.  **Save Old Base Pointer:** Preserve the caller's `%rbp` so it can be restored later.
    * *Instruction:* `pushq %rbp`
2.  **Set New Base Pointer:** specific the current top of the stack as the new base.
    * *Instruction:* `rrmovq %rsp, %rbp`
3.  **Allocate Locals:** Move the stack pointer down to make room for variables.
    * *Instruction:* `subq $SIZE, %rsp`

### **Accessing Data**
* **Local Variables:** Accessed via **negative offsets** from `%rbp` (e.g., `-8(%rbp)`).
* **Arguments:** Accessed via **positive offsets** from `%rbp` (e.g., `16(%rbp)` to skip the saved `%rbp` and return address).

### **Postamble (Teardown)**
1.  **Reset Stack Pointer:** Move `%rsp` back to `%rbp` (effectively discarding locals).
    * *Implicit Action:* `rrmovq %rbp, %rsp` (or simply adding the offset back).
2.  **Restore Old Base Pointer:** Pop the saved `%rbp` back into the register.
    * *Instruction:* `popq %rbp`
3.  **Return:** Execute `ret`.

---

## 5. Caller vs. Callee Responsibilities

### **Caller Responsibilities**
* **Before Call:**
    * Push arguments (reverse order) if they don't fit in registers.
    * Push caller-saved registers if their values need preserving.
    * Execute `call` (pushes return address).
* **After Return:**
    * Remove arguments from the stack (increment `%rsp`).
    * Restore caller-saved registers (if any).

### **Callee Responsibilities**
* **Setup:** Allocate local frame (save `%rbp` if using Method B).
* **Execution:** Perform task, ensuring callee-saved registers (`%rbx`, `%rbp`, etc.) are preserved.
* **Teardown:** Deallocate frame and restore `%rbp` (if used).


    ![[image-2.png]]

    