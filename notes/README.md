# Linear Algebra for Computer Science

**Student:** Jephthah Lorlornyo Mortoti-Agogyi  
**Focus:** Matrix Algebra & Algorithms

---

## 1. Matrix Fundamentals & Notation

### The Definition

A matrix is a rectangular array of numbers described by its size: **$m \times n$**

- **$m$** = Number of Rows (Horizontal)
- **$n$** = Number of Columns (Vertical)

### The (i, j) Indexing System

To address a specific number, we use $a_{ij}$:
- **$i$** = Row index
- **$j$** = Column index

> **⚠️ CRITICAL: The "Off-By-One" Error**
>
> - **Math is 1-indexed:** The top-left number is $a_{11}$
> - **Code is 0-indexed:** The top-left number is `A[0][0]`
> - **Translation:** Math $a_{ij}$ → Code `A[i-1][j-1]`

### Programmer's Intuition: "Database View"

Think of a Matrix (A) as a **Database Table**:
- **Columns:** Represent *Features* or *Fields* (e.g., Age, Price, Speed)
- **Rows:** Represent *Individual Records* (e.g., User 1, User 2)

```python
# A 3×2 Matrix (3 Users, 2 Features)
matrix = [
    [10, 20],  # User 1
    [30, 40],  # User 2
    [50, 60]   # User 3
]
```

### Special Matrices

**1. Zero Matrix (0)**
- All entries are 0
- Acts like the number **0** in arithmetic

**2. Identity Matrix ($I$)**
- Square matrix with **1s** on the diagonal and **0s** everywhere else
- Acts like the number **1** in arithmetic
- Code Logic: `if (i == j) return 1 else return 0`

**3. Diagonal Matrix**
- Non-zero numbers only on the diagonal (i = j)
- All off-diagonal elements are 0

---

## 2. Basic Arithmetic (Addition & Scaling)

### Matrix Addition ($A + B$)

**Rule:** Dimensions must match exactly ($m \times n$ + $m \times n$)

**Logic:** Add cell-by-cell: $C_{ij} = A_{ij} + B_{ij}$

**Code Logic:** A simple nested loop iterating through every element.

```python
# Example
for i in range(rows):
    for j in range(cols):
        C[i][j] = A[i][j] + B[i][j]
```

### Scalar Multiplication ($cA$)

**Rule:** Multiply **every single entry** in the matrix by the scalar number $c$

**Logic:** "Broadcast" the operation to the whole array.

**Code Logic:**
```python
# Logic for Scalar Multiplication
for i in range(rows):
    for j in range(cols):
        Matrix[i][j] *= scalar
```

---

## 3. Matrix Multiplication ($A \times B$)

This is the most complex operation in basic matrix algebra.

### The "API Contract" (Dimensions Rule)

You can only multiply $A \times B$ if the **Inner Dimensions Match**:

$$(m \times \mathbf{n}) \cdot (\mathbf{n} \times p) \rightarrow (m \times p)$$

- **Input:** Columns of A must equal Rows of B
- **Output:** The result takes the "Outer" dimensions ($m \times p$)

### The Algorithm: "Row by Column"

To find the value at position $(i, j)$ in the result:

1. Take **Row $i$** from Matrix A
2. Take **Column $j$** from Matrix B
3. **Dot Product:** Multiply matching pairs and sum them up

$$C_{ij} = \sum_{k} a_{ik} \cdot b_{kj}$$

### Programmer's Intuition: The Triple Loop

Standard Matrix Multiplication is an $O(n^3)$ operation.

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

**1. Not Commutative:** $AB \neq BA$
- Order matters! (Socks then Shoes ≠ Shoes then Socks)

**2. No Cancellation:** If $AB = AC$, you **cannot** assume $B = C$

**3. Zero Product:** $AB = 0$ does **not** mean $A = 0$ or $B = 0$
- Two non-zero matrices can multiply to give the zero matrix

---

## 4. Transposes ($A^T$)

### Definition

Flip the matrix over its main diagonal. Rows become Columns.

- Dimension change: $m \times n \rightarrow n \times m$

### Programmer's Intuition: Index Swapping

```python
Transpose[j][i] = Original[i][j]
```

The row index becomes the column index, and vice versa.

### Properties

**1. Double Flip:** $(A^T)^T = A$
- Transposing twice gives you back the original matrix

**2. Sum:** $(A + B)^T = A^T + B^T$
- You can distribute transpose over addition

**3. The Trap (Product Rule):**
$$(AB)^T = B^T A^T$$

**Logic:** When you reverse the dimensions (transpose), you must also reverse the order of multiplication to keep the "Inner Dimensions" matching.

---

## 5. Inverse of a Matrix ($A^{-1}$)

### 1. The Core Definition

**Requirement:** The matrix MUST be **Square** ($n \times n$). You cannot invert a non-square matrix.

**The Identity Property:** A matrix $A$ is invertible if there exists a matrix $A^{-1}$ such that:
$$A \cdot A^{-1} = I \quad \text{and} \quad A^{-1} \cdot A = I$$

*(Where $I$ is the Identity Matrix containing 1s on the diagonal and 0s elsewhere).*

### 2. Invertible vs. Singular

**Invertible (Non-Singular):** The inverse exists. The determinant is **not zero**.

**Non-Invertible (Singular):** The inverse does **not** exist. The determinant **is zero**.
- *Intuition:* A singular matrix squashes space into a lower dimension (like flattening a 3D box into a 2D sheet), so you can't "reverse" the process.

### 3. The $2 \times 2$ Formula

Given matrix $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$:

**Step 1:** Calculate the Determinant ($\det$).
$$\det(A) = ad - bc$$

**Step 2:** Apply the Formula.
$$A^{-1} = \frac{1}{\det(A)} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$

*Mechanics:* Swap the positions of $a$ and $d$. Change the signs of $b$ and $c$. Divide everything by the determinant.

### 4. Examples

**Example 1: Invertible Matrix**

$$A = \begin{bmatrix} 4 & 7 \\ 2 & 6 \end{bmatrix}$$

1. **Det:** $(4)(6) - (7)(2) = 24 - 14 = 10$.
2. **Rearrange:** Swap diagonals, negate off-diagonals $\rightarrow \begin{bmatrix} 6 & -7 \\ -2 & 4 \end{bmatrix}$.
3. **Final Inverse:**
   $$A^{-1} = \frac{1}{10} \begin{bmatrix} 6 & -7 \\ -2 & 4 \end{bmatrix} = \begin{bmatrix} 0.6 & -0.7 \\ -0.2 & 0.4 \end{bmatrix}$$

**Example 2: Singular Matrix (No Inverse)**

$$B = \begin{bmatrix} 1 & 2 \\ 2 & 4 \end{bmatrix}$$

1. **Det:** $(1)(4) - (2)(2) = 4 - 4 = 0$.
2. **Result:** Since we cannot divide by $0$, $B$ is **Singular** (Non-invertible).

### 💻 Programming Context (Why this matters in CS)

**Division doesn't exist:** In Matrix Algebra, you cannot divide $B / A$. Instead, you multiply by the inverse: $B \cdot A^{-1}$.

**Graphics & Undo Buttons:** In game development, if a Transformation Matrix $T$ moves a character forward, the Inverse Matrix $T^{-1}$ moves them backward. If $T$ is singular, the transformation is destructive (information is lost) and cannot be undone.

**System Solving:** We use inverses to solve linear systems ($Ax = b \rightarrow x = A^{-1}b$), though computationally we usually avoid calculating full inverses for large matrices due to high computational costs.

---

**End of Notes**
