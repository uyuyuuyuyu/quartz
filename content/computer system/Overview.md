## Summary of Common y86-64 Operations

### 1. Data Movement Instructions
These instructions move data between registers, memory, and immediate (constant) values.
* **irmovq V, rB**: Moves an immediate (constant) value `V` into register `rB`.
* **rrmovq rA, rB**: Moves the value of register `rA` into register `rB`.
* **rmmovq rA, D(rB)**: Stores a value from register `rA` into memory at address `D + rB`.
* **mrmovq D(rB), rA**: Loads a value from memory at address `D + rB` into register `rA`.

---

### 2. Arithmetic and Logical Unit (ALU) Operations
All ALU operations perform calculations on two registers, storing the result in the second register (`rB`). These also update the **Condition Flags**.
* **addq**: Adds `rA` to `rB`.
* **subq**: Subtracts `rA` from `rB`.
* **andq**: Performs a bitwise AND.
* **xorq**: Performs a bitwise XOR.

---

### 3. Conditional Flags (Condition Codes)
The y86-64 architecture uses three 1-bit flags to track the status of the most recent ALU operation. These are automatically updated after every arithmetic or logical instruction.
* **Zero Flag (ZF)**: Set to 1 if the result of the last operation was zero.
* **Sign Flag (SF)**: Set to 1 if the last operation resulted in a negative value (the most significant bit is 1).
* **Overflow Flag (OF)**: Set to 1 if a signed arithmetic operation (like `addq` or `subq`) resulted in a two's complement overflow.


---

### 4. Control Flow Instructions
These instructions change the **Program Counter (PC)** to shift the execution path.
* **Jumps (jmp, jXX)**: Used for loops and conditionals. Conditional jumps (e.g., `je`, `jne`, `jl`) only execute if specific condition codes are met.
* **call**: Pushes the return address onto the stack and jumps to a label.
* **ret**: Pops the return address from the stack and jumps back to it.
* **Conditional Moves (cmovXX)**: A variant of `rrmovq` that only copies the value if the specific condition (like "equal" or "less than") is true based on the flags.

---

### 5. Stack Operations
The y86 stack grows toward **lower memory addresses**, and the stack pointer is stored in `%rsp`.
* **pushq rA**: Decrements `%rsp` by 8 and writes the value of `rA` to the new stack top.
* **popq rA**: Reads the value at the current stack top into `rA` and then increments `%rsp` by 8.


### 1. Advanced ALU Operations (Opcode 6)
Beyond basic addition and subtraction, the simulator mentioned in the lecture supports "Bonus" operations:
* **addq (fn 0)**: rB ← rB + rA.
* **subq (fn 1)**: rB ← rB - rA.
* **andq (fn 2)**: rB ← rB & rA.
* **xorq (fn 3)**: rB ← rB ^ rA.
* **mulq (fn 4)**: Multiply rB by rA (Simulator Bonus).
* **divq (fn 5)**: Divide rB by rA (Simulator Bonus).
* **modq (fn 6)**: Remainder of rB / rA (Simulator Bonus).

---

### 2. Conditional Jumps (Opcode 7)
These instructions change the Program Counter (PC) only if the specified condition is met based on the status flags (ZF, SF, OF):
* **jmp (fn 0)**: Unconditional jump.
* **jle (fn 1)**: Jump if less than or equal ( (SF^OF) | ZF ).
* **jl (fn 2)**: Jump if less than ( SF^OF ).
* **je (fn 3)**: Jump if equal ( ZF ).
* **jne (fn 4)**: Jump if not equal ( ~ZF ).
* **jge (fn 5)**: Jump if greater than or equal ( ~(SF^OF) ).
* **jg (fn 6)**: Jump if greater than ( ~(SF^OF) & ~ZF ).

---

### 3. Conditional Moves (Opcode 2)
Conditional moves use the same function codes as jumps but perform a register-to-register move (`rrmovq`) only if the condition is true:
* **rrmovq (fn 0)**: Unconditional register move.
* **cmovle (fn 1)**: Move if less than or equal.
* **cmovl (fn 2)**: Move if less than.
* **cmove (fn 3)**: Move if equal.
* **cmovne (fn 4)**: Move if not equal.
* **cmovge (fn 5)**: Move if greater than or equal.
* **cmovg (fn 6)**: Move if greater than.

---

### 4. System & Control
* **call (Opcode 8)**: Pushes return address and jumps to destination.
* **ret (Opcode 9)**: Pops return address from stack and jumps to it.
* **halt (Opcode 0)**: Stops execution and sets status to HLT.
* **nop (Opcode 1)**: No operation.


# y86-64 Instruction Examples

### 1. Conditional Jumps (Opcode 7)
Jumps are used to create loops and "if-else" structures by changing the **Program Counter (PC)**.

```assembly
    # Example: Simple Loop that counts down from 5 to 0
    irmovq $5, %rax      # Load 5 into %rax
    irmovq $1, %rbx      # Load 1 into %rbx (used for decrementing)

loop:
    subq %rbx, %rax      # %rax = %rax - 1. Updates Condition Codes (ZF, SF, OF)
    jne loop             # Jump if Not Equal (ZF=0): If %rax is not 0, go back to 'loop'
    halt                 # Stop when %rax reaches 0



# Example: Find the maximum of two numbers (RAX and RBX)
    # Result will be stored in %rax
    irmovq $10, %rax     # %rax = 10
    irmovq $20, %rbx     # %rbx = 20
    
    rrmovq %rbx, %rcx    # Copy %rbx to %rcx
    subq %rax, %rcx      # Subtract %rax from %rcx (20 - 10 = 10). Sets SF=0
    
    cmovg %rbx, %rax     # Conditional Move if Greater: If %rbx > %rax, copy %rbx to %rax
    # %rax now contains 20


    main:
    irmovq $0x2000, %rsp # Initialize Stack Pointer to high memory
    irmovq $15, %rax     # Load argument 15
    call square          # Push return address to stack and jump to 'square'
    halt                 # Stop execution

square:
    # (Imagine logic here to multiply %rax by itself)
    # This example uses push/pop to preserve a register
    pushq %rbx           # Save %rbx on the stack; decrements %rsp by 8
    # ... calculation logic ...
    popq %rbx            # Restore %rbx; increments %rsp by 8
    ret                  # Pop return address and jump back to 'main'


```

# pc increment

uint64_t increment_pc(uint64_t pc, y86_icode_t icode) {
    switch(icode) {
        // 1-byte instructions
        case 0x0: // I_HALT
        case 0x1: // I_NOP
        case 0x9: // I_RET
            return pc + 1;

        // 2-byte instructions
        case 0x2: // I_RRMOVQ
        case 0x6: // I_OPQ
        case 0xA: // I_PUSHQ
        case 0xB: // I_POPQ
            return pc + 2;

        // 9-byte instructions
        case 0x7: // I_JXX
        case 0x8: // I_CALL
            return pc + 9;

        // 10-byte instructions
        case 0x3: // I_IRMOVQ
        case 0x4: // I_RMMOVQ
        case 0x5: // I_MRMOVQ
            return pc + 10;
            
        default:
            return pc;
    }
}