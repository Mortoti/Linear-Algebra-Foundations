# Day 01 - Matrix Algebra Fundamentals
**Date:** November 26, 2025  
**Topic:** Matrix Operations, Properties & Common Mistakes

---

## 1. Matrix Addition: Same Size Required

**The Rule:** You can only add matrices that are exactly the same size. Add the numbers in matching positions.

Think of it like adding two identical grids together, cell by cell.

### ✅ Example 1: Adding 2×2 matrices
```
  [1  2]   +   [5  6]   =   [6   8]
  [3  4]       [7  8]       [10 12]
```
**What happened?**
- Top-left: 1 + 5 = 6
- Top-right: 2 + 6 = 8  
- Bottom-left: 3 + 7 = 10
- Bottom-right: 4 + 8 = 12

### ✅ Example 2: Adding column vectors (3×1)
```
  [1]   +   [4]   =   [5]
  [2]       [5]       [7]
  [3]       [6]       [9]
```
Each row adds straight across: 1+4=5, 2+5=7, 3+6=9.

### ❌ Example 3: Mismatched sizes don't work
```
[1  2]   +   [5  6]   =   ERROR!
[3  4]       [7  8]
[5  6]
```
The first matrix is 3×2 (3 rows, 2 columns). The second is 2×2. You can't match them up, so addition is impossible.

### Key Properties
- **Order doesn't matter:** A + B = B + A (just like regular numbers)
- **Grouping doesn't matter:** (A + B) + C = A + (B + C)
- **Zero matrix acts like zero:** A + [0 matrix] = A

---

## 2. Scalar Multiplication: Multiply Everything

**The Rule:** A scalar is just a regular number. When you multiply a matrix by a scalar, multiply every single entry inside the matrix by that number.

### Example 1: Doubling a matrix
```
      [1   5]       [2   10]
  2 × [3   4]   =   [6    8]
```
The 2 multiplies each entry: 2×1=2, 2×5=10, 2×3=6, 2×4=8.

### Example 2: Negating a column vector
```
       [2]        [-2]
 -1 × [-5]   =    [5]
```
Multiplying by -1 flips all the signs.

### Key Properties
- **Distributes over addition:** r(A + B) = rA + rB
- **Scalars can be added first:** (r + s)A = rA + sA  
- **Scalars can be multiplied first:** r(sA) = (rs)A

---

## 3. Matrix Multiplication: The Tricky One

**The Rule:** To multiply matrix A by matrix B, the **number of columns in A** must equal the **number of rows in B**.

The result's size: (rows of A) × (columns of B)

**Think of it like this:** Each entry in the result comes from taking a row from the first matrix and a column from the second matrix, multiplying corresponding entries, then adding them all up.

### Size Check Example
```
(2×3) × (3×2) → Result is (2×2)  ✅
 ^^^^   ^^^^
These two middle numbers (both 3) must match!

(2×2) × (3×2) → Can't multiply  ❌
      ^^  ^^
      2 ≠ 3, so it's impossible
```

### Example 1: Multiplying a 2×3 by a 3×2
```
[1  2  3]       [7  8]
[4  5  6]   ×   [9  1]   =   [Result will be 2×2]
                [2  3]
```

**How to get the top-left entry (first row × first column):**
- Take row 1 from the first matrix: [1, 2, 3]
- Take column 1 from the second matrix: [7, 9, 2]
- Multiply pairs and add: (1×7) + (2×9) + (3×2) = 7 + 18 + 6 = **31**

**How to get the top-right entry (first row × second column):**
- Take row 1 from the first matrix: [1, 2, 3]
- Take column 2 from the second matrix: [8, 1, 3]  
- Multiply pairs and add: (1×8) + (2×1) + (3×3) = 8 + 2 + 9 = **19**

Continue this process for all four positions in the result.

### Example 2: The Identity Matrix (Special!)
```
[5  6]       [1  0]       [5  6]
[7  8]   ×   [0  1]   =   [7  8]
```

The identity matrix (1s on diagonal, 0s elsewhere) is like multiplying by 1 in regular math—it doesn't change anything!

### Key Properties
- **Associative:** (AB)C = A(BC) - grouping doesn't matter
- **Distributive:** A(B + C) = AB + AC - you can distribute
- **⚠️ NOT commutative:** AB ≠ BA - **order matters!** This is the biggest difference from regular number multiplication.

---

## 4. ⚠️ Common Traps That Will Get You

These mistakes feel natural because they work with regular numbers, but matrices break these rules.

### Trap 1: The Square Trap
```
❌ WRONG:  (A + B)² = A² + 2AB + B²
✅ RIGHT:  (A + B)² = A² + AB + BA + B²
```

**Why?** When you expand (A+B)(A+B), you get AA + AB + BA + BB. Since AB ≠ BA for matrices, you can't combine AB and BA into 2AB.

**Real numbers work differently:** With numbers, (3+2)² = 3² + 2(3)(2) + 2² because 3×2 = 2×3.

### Trap 2: The Power Trap  
```
❌ WRONG:  (AB)² = A²B²
✅ RIGHT:  (AB)² = ABAB
```

**Why?** (AB)² means (AB)(AB) = ABAB. You can't rearrange the middle BA to A²B² because order matters in matrix multiplication.

### Trap 3: The Cancellation Trap
```
If AB = AC, you CANNOT conclude that B = C
```

**Why?** In regular algebra, if 5x = 5y, you can divide both sides by 5 to get x = y. But with matrices, you can't "divide" by A. Some matrices behave strangely when multiplied (we'll learn about "invertibility" later).

**Example where it fails:**
```
Let A = [1  1]    B = [1]    C = [0]
        [0  0]        [0]        [1]

AB = [1]  and  AC = [1]  (they're equal!)
     [0]            [0]

But B ≠ C (obviously [1,0] ≠ [0,1])
```

---

## 5. The Transpose: Flip Rows and Columns

**The Rule:** Rows become columns, columns become rows. The matrix dimensions flip from m×n to n×m.

Think of it like rotating the matrix 90° and then flipping it.

### Example 1: Flipping a 2×2 matrix
```
Original:         Transpose:
[1  2]            [1  3]
[3  4]            [2  4]

Row 1 [1,2] becomes column 1 [1,3]
Row 2 [3,4] becomes column 2 [2,4]
```

### Example 2: Flipping a 3×2 into a 2×3
```
Original (3×2):         Transpose (2×3):
[1  2]                  [1  3  5]
[3  4]                  [2  4  6]
[5  6]
```

**What happened?**
- Row 1 (1,2) → Column 1 (1,2)
- Row 2 (3,4) → Column 2 (3,4)
- Row 3 (5,6) → Column 3 (5,6)

### Key Property
**(A^T)^T = A** - Transposing twice brings you back to where you started.

---

## Quick Reference: Can I Do This Operation?

| Operation | Requirement | Example |
|-----------|-------------|---------|
| A + B | Same size | (2×3) + (2×3) ✅ |
| rA (scalar × matrix) | None | Works with any matrix ✅ |
| AB | Columns of A = Rows of B | (2×**3**)()**3**×4) ✅ |
| A^T | None | Any matrix can be transposed ✅ |


# Matrix Fundamentals & Notation

## 1. The Matrix & Indexing $(i, j)$

A matrix is essentially a **2D Array** or a grid of numbers. We describe its size as $m \times n$.
* $m$ = Number of **Rows** (Horizontal lines)
* $n$ = Number of **Columns** (Vertical lines)

### The $(i, j)$ Notation
To pinpoint a specific number in matrix $A$, we use the coordinates $a_{ij}$.
* $i$ represents the **Row** number.
* $j$ represents the **Column** number.

$$
A = \begin{bmatrix}
a_{11} & a_{12} & a_{13} \\
a_{21} & a_{22} & a_{23} \\
a_{31} & a_{32} & a_{33}
\end{bmatrix}
$$

> **⚠️ CRITICAL WARNING FOR PROGRAMMERS**
> * **Math is 1-indexed:** The first element is $a_{11}$ (Row 1, Col 1).
> * **Code is 0-indexed:** In Python/C++, the first element is `A[0][0]`.
> * *Translation:* Math $a_{ij}$ $\rightarrow$ Code `A[i-1][j-1]`

---

## 2. Column Vectors (The "Data" View)

We can think of a matrix $A$ not just as a block of numbers, but as a collection of **Column Vectors** standing side-by-side.

$$
A = [\vec{a}_1 \quad \vec{a}_2 \quad \dots \quad \vec{a}_n]
$$

### Programmer's Intuition
Think of the matrix as a **Database Table**:
* The **Matrix** is the whole table.
* Each **Column Vector** ($\vec{a}_j$) is a specific field (e.g., `[Age]`, `[Height]`, `[Salary]`).
* Each **Row** is a specific user record.

```python
# A 3x3 Matrix viewed as 3 Column Vectors
col_1 = [1, 4, 7]
col_2 = [2, 5, 8]
col_3 = [3, 6, 9]

matrix = [col_1, col_2, col_3] # Grouping vectors

