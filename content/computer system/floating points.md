# Chapter 3.5: Floating Point

## 1. Introduction to Real Numbers
- **Reals:** Numbers with fractions, distinct from signed/unsigned integers.
- **Scientific Notation:** A single digit to the left of the decimal point (e.g., $1.0_{ten} \times 10^{-9}$).
- **Normalized Number:** A number in floating-point notation that has no leading 0s.
- **Binary Scientific Notation:** Uses a base of 2 (e.g., $1.0_{two} \times 2^{-1}$).
    - **Binary Point:** The binary equivalent of a decimal point.
    - **Floating Point:** Computer arithmetic where the binary point is not fixed.

## 2. IEEE 754 Floating-Point Representation
Standard notation for reals helps exchange data, simplifies algorithms, and increases accuracy.

### The Formula
Normalized floating-point numbers follow this form:
$$(-1)^{S} \times (1 + \text{Fraction}) \times 2^{(\text{Exponent} - \text{Bias})}$$ 
- **Sign (S):** 1 bit. Determines positive or negative.
- **Fraction (Significand):** The value between 0 and 1. The leading 1 is **implicit** (hidden) to pack more bits.
- **Exponent (Biased):** Uses a bias to allow integers to represent negative exponents easily for sorting.


![[bitRepresentation.png]]

### Formats
| Feature | Single Precision (float) | Double Precision (double) |
| :--- | :--- | :--- |
| **Word Size** | 32 bits  | 64 bits  |
| **Sign Bit** | 1 bit  | 1 bit  |
| **Exponent** | 8 bits  | 11 bits  |
| **Fraction** | 23 bits  | 52 bits  |
| **Bias** | 127  | 1023  |
| **Range** | $\approx \pm 10^{\pm 38}$  | $\approx \pm 10^{\pm 308}$  |

### Special Values
- **Zero:** Represented by an exponent of 0 and fraction of 0.
- **Infinity ($\infty$):** Result of dividing by 0 or overflow. Uses the largest exponent value.
- **NaN (Not a Number):** Represents invalid operations (e.g., $0/0$). Uses the largest exponent with a non-zero fraction.
- **Denormalized Numbers:** Have an exponent of 0 and non-zero fraction. Used for "gradual underflow" near 0 .

## 3. Exceptions
- **Overflow:** Positive exponent is too large to fit in the field.
- **Underflow:** Negative exponent is too large (number too small) to fit.
- **Handling:** RISC-V does not raise exceptions (interrupts) for these; instead, software checks the floating-point control and status register (fcsr).

## 4. Arithmetic Algorithms

### Floating-Point Addition 
1.  **Compare Exponents:** Shift the significand of the smaller number right until its exponent matches the larger number.
2.  **Add:** Add the significands.
3.  **Normalize:** Shift sum left or right and adjust exponent. Check for overflow/underflow.
4.  **Round:** Round significand to appropriate bits.
5.  **Re-normalize:** If rounding unnormalized the number, repeat step 3.

### Floating-Point Multiplication 
1.  **Add Exponents:** Add biased exponents and **subtract the bias** once to get the new biased exponent.
2.  **Multiply:** Multiply the significands.
3.  **Normalize:** Shift product right and increment exponent if necessary.
4.  **Check:** Check for overflow/underflow.
5.  **Round:** Round significand.
6.  **Sign:** Set result sign (positive if operands match, negative if they differ).

## 5. RISC-V Floating-Point Architecture

### Registers
- **32 Registers:** `f0` – `f31`.
- **f0 Behavior:** Unlike integer `x0`, `f0` is **not** hard-wired to 0.
- **Size:** Can hold single (32-bit) or double (64-bit) values.

### Instruction Set 
| Operation | Single Precision | Double Precision |
| :--- | :--- | :--- |
| **Arithmetic** | `fadd.s`, `fsub.s`, `fmul.s`, `fdiv.s`, `fsqrt.s` | `fadd.d`, `fsub.d`, `fmul.d`, `fdiv.d`, `fsqrt.d` |
| **Comparison** | `feq.s`, `flt.s`, `fle.s` | `feq.d`, `flt.d`, `fle.d` |
| **Load** | `flw` (Float Load Word) | `fld` (Float Load Double) |
| **Store** | `fsw` (Float Store Word) | `fsd` (Float Store Double) |

*Note: Comparisons write an integer result (0 or 1) to an integer register for branching.*

## 6. Accuracy and Rounding
Floating-point numbers are approximations. Accuracy is often measured in **ulps** (units in the last place).

### Hardware Support
To ensure accurate rounding, hardware keeps extra bits during intermediate calculations:
1.  **Guard Bit:** First extra bit on the right.
2.  **Round Bit:** Second extra bit on the right.
3.  **Sticky Bit:** Set if *any* non-zero bits exist to the right of the round bit.

### Rounding Modes
- **Round to Nearest Even:** Standard mode. If halfway, round to the nearest even number (least significant bit becomes 0).
- **Fused Multiply Add (FMA):** Performs $a = a + (b \times c)$ with only **one** rounding step, improving precision.

## ieee 754 simulator

[link](https://www.h-schmidt.net/FloatConverter/IEEE754.html)

## 7. Programming Example: C to Assembly
**C Code:**
```c
float f2c (float fahr) {
  return ((5.0f/9.0f) * (fahr - 32.0f));
}