# Linear Algebra for Computer Science

**Student:** Jephthah Lorlornyo Mortoti-Agogyi  
**Focus:** Matrix Algebra & Algorithms

---

## 1. Matrix Fundamentals & Notation

### The Definition

A matrix is a rectangular array of numbers described by its size: **m × n**

- **m** = Number of Rows (Horizontal)
- **n** = Number of Columns (Vertical)

**Example:**
```
A = [1  2  3]  ← 2 rows, 3 columns = 2×3 matrix
    [4  5  6]
```

### The (i, j) Indexing System

To address a specific number, we use a<sub>ij</sub>:
- **i** = Row index
- **j** = Column index

**Example:**
```
A = [10  20  30]
    [40  50  60]

a₁₁ = 10 (row 1, col 1)
a₁₂ = 20 (row 1, col 2)
a₂₃ = 60 (row 2, col 3)
```

> **⚠️ CRITICAL: The "Off-By-One" Error**
>
> - **Math is 1-indexed:** The top-left number is a₁₁
> - **Code is 0-indexed:** The top-left number is `A[0][0]`
> - **Translation:** Math a<sub>ij</sub> → Code `A[i-1][j-1]`

**Example in Code:**
```python
# Math: a₂₃ means row 2, column 3
# Code: A[1][2] (subtract 1 from each index)

matrix = [[10, 20, 30],
          [40, 50, 60]]

# To get a₂₃ (which is 60):
value = matrix[1][2]  # NOT matrix[2][3]!
```

### Programmer's Intuition: "Database View"

Think of a Matrix (A) as a **Database Table**:
- **Columns:** Represent *Features* or *Fields* (e.g., Age, Price, Speed)
- **Rows:** Represent *Individual Records* (e.g., User 1, User 2)

```python
# A 3×2 Matrix (3 Users, 2 Features)
#        Age  Salary
matrix = [
    [25,  50000],  # User 1
    [30,  60000],  # User 2
    [35,  70000]   # User 3
]
```

### Special Matrices

**1. Zero Matrix (0)**
- All entries are 0
- Acts like the number **0** in arithmetic

```
[0  0  0]
[0  0  0]
```

**2. Identity Matrix (I)**
- Square matrix with **1s** on the diagonal and **0s** everywhere else
- Acts like the number **1** in arithmetic
- Code Logic: `if (i == j) return 1 else return 0`

```
[1  0  0]
[0  1  0]
[0  0  1]
```

**3. Diagonal Matrix**
- Non-zero numbers only on the diagonal (i = j)
- All off-diagonal elements are 0

```
[5  0  0]
[0  3  0]
[0  0  8]
```

---

## 2. Basic Arithmetic (Addition & Scaling)

### Matrix Addition (A + B)

**Rule:** Dimensions must match exactly (m × n + m × n)

**Logic:** Add cell-by-cell: C<sub>ij</sub> = A<sub>ij</sub> + B<sub>ij</sub>

**Example:**
```
[1  2]   [5  6]   [1+5  2+6]   [6   8]
[3  4] + [7  8] = [3+7  4+8] = [10  12]
```

**Code Logic:**
```python
# Simple nested loop
for i in range(rows):
    for j in range(cols):
        C[i][j] = A[i][j] + B[i][j]
```

### Scalar Multiplication (cA)

**Rule:** Multiply **every single entry** in the matrix by the scalar number c

**Logic:** "Broadcast" the operation to the whole array.

**Example:**
```
    [1  2]   [3×1  3×2]   [3   6]
3 × [3  4] = [3×3  3×4] = [9  12]
```

**Code Logic:**
```python
# Logic for Scalar Multiplication
for i in range(rows):
    for j in range(cols):
        Matrix[i][j] *= scalar
```

---

## 3. Matrix Multiplication (A × B)

This is the most complex operation in basic matrix algebra.

### The "API Contract" (Dimensions Rule)

You can only multiply A × B if the **Inner Dimensions Match**:

**(m × n) · (n × p) → (m × p)**

- **Input:** Columns of A must equal Rows of B
- **Output:** The result takes the "Outer" dimensions (m × p)

**Example of Dimension Checking:**
```
A: 3×4  ×  B: 4×2  =  C: 3×2  ✓ Valid (inner 4s match)
A: 2×3  ×  B: 5×2  =  ✗ Invalid (3 ≠ 5)
```

### The Algorithm: "Row by Column"

To find the value at position (i, j) in the result:

1. Take **Row i** from Matrix A
2. Take **Column j** from Matrix B
3. **Dot Product:** Multiply matching pairs and sum them up

**Formula:** C<sub>ij</sub> = Σ(a<sub>ik</sub> · b<sub>kj</sub>)

**Step-by-Step Example:**
```
A = [1  2]    B = [5  6]
    [3  4]        [7  8]

Find C₁₁ (row 1, col 1):
• Row 1 of A: [1, 2]
• Col 1 of B: [5, 7]
• Multiply pairs: (1×5) + (2×7) = 5 + 14 = 19

Find C₁₂ (row 1, col 2):
• Row 1 of A: [1, 2]
• Col 2 of B: [6, 8]
• Multiply pairs: (1×6) + (2×8) = 6 + 16 = 22

Find C₂₁ (row 2, col 1):
• Row 2 of A: [3, 4]
• Col 1 of B: [5, 7]
• Multiply pairs: (3×5) + (4×7) = 15 + 28 = 43

Find C₂₂ (row 2, col 2):
• Row 2 of A: [3, 4]
• Col 2 of B: [6, 8]
• Multiply pairs: (3×6) + (4×8) = 18 + 32 = 50

Result: C = [19  22]
            [43  50]
```

### Programmer's Intuition: The Triple Loop

Standard Matrix Multiplication is an O(n³) operation.

```python
# C = A × B
for i in range(A.rows):           # Select Row from A
    for j in range(B.cols):       # Select Column from B
        sum = 0
        for k in range(A.cols):   # Iterate the matching elements
            sum += A[i][k] * B[k][j]
        C[i][j] = sum
```

### ⚠️ The "Gotchas" (Where Math Breaks From Intuition)

**1. Not Commutative:** AB ≠ BA
- Order matters! (Socks then Shoes ≠ Shoes then Socks)

**Example:**
```
A = [1  2]    B = [5  6]
    [3  4]        [7  8]

A×B = [19  22]    B×A = [23  34]  ← Different results!
      [43  50]          [31  46]
```

**2. No Cancellation:** If AB = AC, you **cannot** assume B = C

**3. Zero Product:** AB = 0 does **not** mean A = 0 or B = 0
- Two non-zero matrices can multiply to give the zero matrix

**Example:**
```
A = [1   2]    B = [ 2  0]    AB = [0  0]
    [2   4]        [-1  0]         [0  0]

Both A and B are non-zero, but AB is the zero matrix!
```

---

## 4. Transposes (A<sup>T</sup>)

### Definition

Flip the matrix over its main diagonal. Rows become Columns.

- Dimension change: m × n → n × m

**Example:**
```
A = [1  2  3]    A^T = [1  4]
    [4  5  6]          [2  5]
                       [3  6]

Rows became columns!
```

### Programmer's Intuition: Index Swapping

```python
Transpose[j][i] = Original[i][j]
```

The row index becomes the column index, and vice versa.

**Code Example:**
```python
original = [[1, 2, 3],
            [4, 5, 6]]

# Create transpose
transpose = [[1, 4],
             [2, 5],
             [3, 6]]

# How: transpose[0][1] = original[1][0] = 4
```

### Properties

**1. Double Flip:** (A<sup>T</sup>)<sup>T</sup> = A
- Transposing twice gives you back the original matrix

**2. Sum:** (A + B)<sup>T</sup> = A<sup>T</sup> + B<sup>T</sup>
- You can distribute transpose over addition

**3. The Trap (Product Rule):**
**(AB)<sup>T</sup> = B<sup>T</sup>A<sup>T</sup>**

**Logic:** When you reverse the dimensions (transpose), you must also reverse the order of multiplication to keep the "Inner Dimensions" matching.

**Example:**
```
A: 2×3  ×  B: 3×4  =  C: 2×4

After transpose:
A^T: 3×2   B^T: 4×3

To multiply these transposed matrices:
B^T × A^T: (4×3) × (3×2) = 4×4  ✓ Works!
A^T × B^T: (3×2) × (4×3) = ✗ Can't multiply (2 ≠ 4)
```

---

## 5. Inverse of a Matrix (A<sup>-1</sup>)

### 1. The Core Definition

**Requirement:** The matrix MUST be **Square** (n × n). You cannot invert a non-square matrix.

**The Identity Property:** A matrix A is invertible if there exists a matrix A<sup>-1</sup> such that:

**A · A<sup>-1</sup> = I  and  A<sup>-1</sup> · A = I**

*(Where I is the Identity Matrix containing 1s on the diagonal and 0s elsewhere).*

### 2. Invertible vs. Singular

**Invertible (Non-Singular):** The inverse exists. The determinant is **not zero**.

**Non-Invertible (Singular):** The inverse does **not** exist. The determinant **is zero**.
- *Intuition:* A singular matrix squashes space into a lower dimension (like flattening a 3D box into a 2D sheet), so you can't "reverse" the process.

### 3. The 2 × 2 Formula

Given matrix:
```
A = [a  b]
    [c  d]
```

**Step 1:** Calculate the Determinant (det).

**det(A) = ad - bc**

**Step 2:** Apply the Formula.

```
A^(-1) = 1/det(A) × [d  -b]
                     [-c  a]
```

*Mechanics:* Swap the positions of a and d. Change the signs of b and c. Divide everything by the determinant.

### 4. Examples

**Example 1: Invertible Matrix**

```
A = [4  7]
    [2  6]
```

**Step 1 - Calculate Det:**
```
det = (4)(6) - (7)(2)
det = 24 - 14
det = 10
```

**Step 2 - Swap and Negate:**
```
[d  -b]   [6  -7]
[-c  a] = [-2  4]
```

**Step 3 - Divide by Det:**
```
A^(-1) = 1/10 × [6  -7]  =  [0.6  -0.7]
                [-2  4]      [-0.2  0.4]
```

**Verification:**
```
A × A^(-1) = [4  7] × [0.6  -0.7]
             [2  6]   [-0.2  0.4]

Row 1, Col 1: (4×0.6) + (7×-0.2) = 2.4 - 1.4 = 1 ✓
Row 1, Col 2: (4×-0.7) + (7×0.4) = -2.8 + 2.8 = 0 ✓
Row 2, Col 1: (2×0.6) + (6×-0.2) = 1.2 - 1.2 = 0 ✓
Row 2, Col 2: (2×-0.7) + (6×0.4) = -1.4 + 2.4 = 1 ✓

Result = [1  0] = Identity Matrix ✓
         [0  1]
```

**Example 2: Singular Matrix (No Inverse)**

```
B = [1  2]
    [2  4]
```

**Step 1 - Calculate Det:**
```
det = (1)(4) - (2)(2)
det = 4 - 4
det = 0
```

**Result:** Since we cannot divide by 0, B is **Singular** (Non-invertible).

**Why?** Notice that row 2 is just 2 × row 1. The rows are not independent, so the matrix collapses space and cannot be reversed.

### 💻 Programming Context (Why this matters in CS)

**Division doesn't exist:** In Matrix Algebra, you cannot divide B / A. Instead, you multiply by the inverse: B · A<sup>-1</sup>.

**Graphics & Undo Buttons:** In game development, if a Transformation Matrix T moves a character forward, the Inverse Matrix T<sup>-1</sup> moves them backward. If T is singular, the transformation is destructive (information is lost) and cannot be undone.

**Example:**
```python
# Move character
new_position = T × old_position

# Undo movement (only works if T is invertible)
old_position = T^(-1) × new_position
```

