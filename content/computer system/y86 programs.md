# Y86-64 Assembly Programming Notes

## 1. Iterative Summation (Loops)

### C Implementation
An iterative function to sum elements in an array.

```c
long sum(long *start, long count)
{
    long sum = 0;
    while (count) {
        sum += *start;
        start++;
        count--;
    }
    return sum;
}
```

### Y86-64 Implementation Details
**Key Difference from x86-64:**
* Y86-64 cannot use immediate data in arithmetic instructions (e.g., `addq $8, %rdi` is invalid).
* Constants must be loaded into registers first (e.g., `irmovq $8, %r8`).

### Full Assembly Dump
```asm
| # Execution begins at address 0
.pos 0
irmovq stack, %rsp      # Set up stack pointer
call main               # Execute main program
halt                    # Terminate program

| # Array of 4 elements
.align 8
array:
.quad 0x000d000d000d
.quad 0x00c000c000c0
.quad 0x0b000b000b00
.quad 0xa000a000a000

| # Main function
main:
irmovq array,%rdi       # Argument 1: start pointer
irmovq $4,%rsi          # Argument 2: count
call sum                # sum(array, 4)
ret

| # long sum(long *start, long count)
| # start in %rdi, count in %rsi
sum:
irmovq $8,%r8           # Constant 8
irmovq $1,%r9           # Constant 1
xorq %rax,%rax          # sum = 0
andq %rsi,%rsi          # Set CC (Condition Codes)
jmp test                # Goto test

loop:
mrmovq (%rdi),%r10      # Get *start
addq %r10,%rax          # Add to sum
addq %r8,%rdi           # start++
subq %r9,%rsi           # count--. Set CC

test:
jne loop                # Stop when 0
ret                     # Return

| # Stack starts here and grows to lower addresses
.pos 0x200
stack:
```

---

## 2. Recursive Product

### C Implementation
A recursive function to calculate the product of an array.

```c
long rproduct(long *start, long count)
{
    if (count <= 1)
        return 1;
    return *start * rproduct(start+1, count-1);
}
```

### Y86-64 Implementation Details
* **Callee-Saved Register (`%rbx`):** The code uses `%rbx` to hold the value of `*start`. Because `%rbx` is a callee-saved register, we must push it to the stack before using it and pop it back before returning. This ensures the value is preserved across the recursive `call rproduct`.

### Assembly Code Snippet
```asm
# long rproduct(long *start, long count)
# start in %rdi, count in %rsi
rproduct:
    xorq   %rax,%rax      # Set return value
    andq   %rsi,%rsi      # Set condition codes
    je     return         # If count <= 0, return
    pushq  %rbx           # Save callee-saved register

    mrmovq (%rdi),%rbx    # Get *start
    irmovq $-1,%r10
    addq   %r10,%rsi      # count--
    irmovq $8,%r10
    addq   %r10,%rdi      # start++
    
    call   rproduct       # Recursive call
    
    imulq  %rbx,%rax      # Multiply *start to product
    popq   %rbx           # Restore callee-saved register

return:
    ret
```





![[image-5.png)]]


the actual write happens at the start of next cycle