# Linear Algebra for Computer Science

**Student:** Jephthah Lorlornyo Mortoti-Agogyi  
**Focus:** Matrix Algebra & Algorithms

---

## 1. Matrix Fundamentals & Notation

### The Definition

A matrix is a rectangular array of numbers described by its size: **$m \times n$**

- **$m$** = Number of Rows (Horizontal)
- **$n$** = Number of Columns (Vertical)

### The $(i, j)$ Indexing System

To address a specific number, we use $a_{ij}$:
- **$i$** = Row index
- **$j$** = Column index

> **⚠️ CRITICAL: The "Off-By-One" Error**
>
> - **Math is 1-indexed:** The top-left number is $a_{11}$
> - **Code is 0-indexed:** The top-left number is `A[0][0]`
> - **Translation:** Math $a_{ij}$ → Code `A[i-1][j-1]`

### Programmer's Intuition: "Database View"

Think of a Matrix ($A$) as a **Database Table**:
- **Columns ($\vec{a}_j$):** Represent *Features* or *Fields* (e.g., Age, Price, Speed)
- **Rows ($\vec{a}_i$):** Represent *Individual Records* (e.g., User 1, User 2)

```python
# A 3×2 Matrix (3 Users, 2 Features)
matrix = [
    [10, 20],  # User 1
    [30, 40],  # User 2
    [50, 60]   # User 3
]
```

### Special Matrices

**1. Zero Matrix ($0$)**
- All entries are 0
- Acts like the number **0**

**2. Identity Matrix ($I$)**
- Square matrix with **1s** on the diagonal and **0s** everywhere else
- Acts like the number **1**
- Code Logic: `if (i == j) return 1 else return 0`

**3. Diagonal Matrix**
- Non-zero numbers only on the diagonal ($i = j$)

---

## 2. Basic Arithmetic (Addition & Scaling)

### Matrix Addition ($A + B$)

**Rule:** Dimensions must match exactly ($m \times n$ + $m \times n$)

**Logic:** Add cell-by-cell: $C_{ij} = A_{ij} + B_{ij}$

**Code Logic:** A simple nested loop iterating through every item.

### Scalar Multiplication ($cA$)

**Rule:** Multiply **every single entry** in the matrix by the scalar number $c$

**Logic:** "Broadcast" the operation to the whole array.

**Code Logic:**
```python
# Logic for Scalar Mult
for i in range(rows):
    for j in range(cols):
        Matrix[i][j] *= scalar
```

---

## 3. Matrix Multiplication ($A \times B$)

This is the most complex operation in basic matrix algebra.

### The "API Contract" (Dimensions Rule)

You can only multiply $A \times B$ if the **Inner Dimensions Match**:

$$
(m \times \mathbf{n}) \cdot (\mathbf{n} \times p) \rightarrow (m \times p)
$$

- **Input:** Columns of A must equal Rows of B
- **Output:** The result takes the "Outer" dimensions ($m \times p$)

### The Algorithm: "Row by Column"

To find the value at position $(i, j)$ in the result:

1. Take **Row $i$** from Matrix A
2. Take **Column $j$** from Matrix B
3. **Dot Product:** Multiply matching pairs and sum them up

$$
(AB)_{ij} = \sum_{k} a_{ik} \cdot b_{kj}
$$

### Programmer's Intuition: The Triple Loop

Standard Matrix Multiplication is an $O(n^3)$ operation.

```python
# C = A * B
for i in range(A.rows):           # Select Row from A
    for j in range(B.cols):       # Select Col from B
        sum = 0
        for k in range(A.cols):   # Iterate the matching elements
            sum += A[i][k] * B[k][j]
        C[i][j] = sum
```

### ⚠️ The "Gotchas" (Where Math Breaks)

**1. Not Commutative:** $AB \neq BA$
- Order matters! (Socks then Shoes ≠ Shoes then Socks)

**2. No Cancellation:** If $AB = AC$, you **cannot** assume $B = C$

**3. Zero Product:** $AB = 0$ does **not** mean $A = 0$ or $B = 0$

---

## 4. Transposes ($A^T$)

### Definition

Flip the matrix over its main diagonal. Rows become Columns.

- Dimension change: $m \times n \rightarrow n \times m$

### Programmer's Intuition: Index Swapping

```python
Transpose[j][i] = Original[i][j]
```

### Properties

**1. Double Flip:** $(A^T)^T = A$

**2. Sum:** $(A + B)^T = A^T + B^T$

**3. The Trap (Product Rule):**
$$
(AB)^T = B^T A^T
$$

**Logic:** When you reverse the dimensions (transpose), you must also reverse the order of multiplication to keep the "Inner Dimensions" matching.
