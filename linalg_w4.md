# Matrices make linear mapping
In Module 4, we continue our discussion of matrices; first we think about how to code up matrix multiplication and matrix operations using the Einstein Summation Convention, which is a widely used notation in more advanced linear algebra courses. Then, we look at how matrices can transform a description of a vector from one basis (set of axes) to another. This will allow us to, for example, figure out how to apply a reflection to an image and manipulate images. We'll also look at how to construct a convenient basis vector set in order to do such transformations. Then, we'll write some code to do these transformations and apply this work computationally.
## Learning Objectives
- Identify matrices as operators
- Relate the transformation matrix to a set of new basis vectors
- Formulate code for mappings based on these transformation matrices
- Write code to find an orthonormal basis set computationally
# Matrices as objects that map one vector onto another; all the types of matrices
## Einstein summation convention and the symmetry of the dot product
We have two matrices, $A$ and $B$.
$$
A = \begin{bmatrix}{}
	a_{11} & a_{12} & ... & a_{1n} \\
	a_{21} & a_{22} & ... & a_{2n} \\
	... & ... & ... & ... \\
	a_{n1} & ... & ... & a_{nn}
	\end{bmatrix}
$$
$$
B = \begin{bmatrix}{}
	b_{11} & b_{12} & ... & b_{1n} \\
	b_{21} & b_{22} & ... & b_{2n} \\
	... & ... & ... & ... \\
	b_{n1} & ... & ... & b_{nn}
	\end{bmatrix}
$$
The product of $A$ and $B$ is $AB$. To calculate a specific element of $AB$, $(ab)_{23}$, we need the dot product between the second row of $A$ and third column of $B$.
$$(ab)_{23} = a_2 b_3 = a_{21}b_{13} + a_{22}b_{23} + ... + a_{2n}b_{n3}$$
Generalized as:
$$(ab)_{jk} = \sum_{j} a_{ij} b_{jk}$$
In shorter conventional form:
$$(ab)_{jk} = a_{ij} b_{jk}$$
This works for multiplying matrices with different shape (non-square matrices), but the number of columns of the first must match the number of rows of the second. The resulting matrix will have the same number of rows of the first and same number of column of the second.
I.e. we can multiply a $m × n$ matrix with an $n × k$ to get a $m × k$ matrix.
# Matrices transform into the new basis vector set
## Matrices changing basis
The columns of a transformation matrix are the axes of the new basis vectors of the mapping in my coordinate system.
Let $r$'s basis vectors being $e_1 = \begin{bmatrix}{} 1 \\ 0 \end{bmatrix}$ and $e_2 = \begin{bmatrix}{} 0 \\ 1 \end{bmatrix}$. To transform $r$ to the new basis $a_1 = \begin{bmatrix}{} 3 \\ 1 \end{bmatrix}$ and $a_2 = \begin{bmatrix}{} 1 \\ 1 \end{bmatrix}$, we calculate the product between $r$ and $A = \begin{bmatrix}{} 3&1\\1&1 \end{bmatrix}$:
$$r' = Ar$$
NB: $A$'s basis are referred to *my* frame, that is the coordinate system of $r$, and $r'$ will be in *my* frame, because it results from the transformation of $r$ through $A$.
To know *my* basis in $A$'s frame, we need to calculate the inverse of $A$, $A^{-1}$, which corresponds to the coordinates of $A$ in $A$'s frame.
So, to calculate back $r$ from $r'$:
$$r = A^{-1}r'$$
In general, any vector $r$ transformed to the basis of a coordinate system $A$ is equal to $Ar$. Both $r$ and $A$ are expressed in the current coordinate system (e.g. $\begin{bmatrix}{} 1&0\\0&1 \end{bmatrix}$). $Ar$ coordinates in $A$'s coordinate system are equal to $A^{-1}rA$.
$$A^{-1}rA = r_A$$
![[Pasted image 20231231151133.png]]
# Making Multiple Mappings, deciding if these are reversible
## Orthogonal matrices
Let's find the transpose of a matrix $A$ (that is, swap rows with columns):
$$A^T_{ij} = A_{ji}$$
Let $A = \begin{bmatrix}{}1&2\\3&4\end{bmatrix}$:
$$\begin{bmatrix}{}1&2\\3&4\end{bmatrix}^T = \begin{bmatrix}{}1&3\\2&4\end{bmatrix}$$
The diagonal elements $1$ and $4$ remain the same, while the other two interchange each other.
In general, for a $m×n$ matrix $A$, the transpose $A^T$ will be a $n×m$ matrix with $A$'s rows becoming $A^T$'s columns.
The product $A^TA$ will always be the identity matrix:
$$A^TA = I$$
Which means:
$$A^T = A^{-1}$$
This is only true if the set of vector composing the matrix are an **orthonormal basis set**, i.e. a set of vectors all perpendicular to each other. The matrix they compose is an **orthogonal matrix**.
