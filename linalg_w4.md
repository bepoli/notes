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
# Recognising mapping matrices and applying these to data
## The Gram–Schmidt process
If I have a set of vectors that are not orthogonal to each other, I can construct an orthonormal basis out of them using the Gram-Schmidt process.
Given the set $v$ of not orthonormal vectors $v_1$, $v_2$, $v_3$, ... $v_n$, we can find a set $e$ of orthonormal vectors out of each.
First we find the unit vector out of $v_1$ by normalising it.
$$e_1 = \frac{v_1}{|v_1|}$$
```python
numpy.linalg.norm(v1)
```
To find $e_2$, we need to extract two components from $v_2$, that are the two vectors that compose the two cathetus with $v_2$ being the hipotenusa. One can be found by calculating the projection of $v_2$ onto $e_1$, and the other, that we call $u_2$, by taking this vector out of $v_2$.
$$v_2 = (v_2 · e_1)\frac{e1}{|e1|} + u_2$$
Since $e_1$ is a unit vector, $|e_1| = 1$.
$$u_2 = v_2 - (v_2 · e_1)e_1$$
Finally, we find the unit vector of $u_2$ to get $e_2$:
$$e_2 = \frac{u_2}{|u_2|}$$
We proceed to find the vector $v_3$. This time, we find the projection of $v_3$ onto the plane defined by $v_1$ and $v_2$.
$$u_3 = v_3 - (v_3 · e_1)e_1 - (v_3 · e_2)e_2$$
$$e_3 = \frac{u_3}{|u_3|}$$
Like this, we find the set of orthonormal vectors $e$.
### Gram-Schmidt procedure on Python
```python
# GRADED FUNCTION
import numpy as np
import numpy.linalg as la

verySmallNumber = 1e-14 # That's 1×10⁻¹⁴ = 0.00000000000001

# Our first function will perform the Gram-Schmidt procedure for 4 basis vectors.
# We'll take this list of vectors as the columns of a matrix, A.
# We'll then go through the vectors one at a time and set them to be orthogonal
# to all the vectors that came before it. Before normalising.
# Follow the instructions inside the function at each comment.
# You will be told where to add code to complete the function.
def gsBasis4(A) :
    B = np.array(A, dtype=np.float_) # Make B as a copy of A, since we're going to alter it's values.
    # The zeroth column is easy, since it has no other vectors to make it normal to.
    # All that needs to be done is to normalise it. I.e. divide by its modulus, or norm.
    B[:, 0] = B[:, 0] / la.norm(B[:, 0])
    # For the first column, we need to subtract any overlap with our new zeroth vector.
    B[:, 1] = B[:, 1] - B[:, 1] @ B[:, 0] * B[:, 0]
    # If there's anything left after that subtraction, then B[:, 1] is linearly independant of B[:, 0]
    # If this is the case, we can normalise it. Otherwise we'll set that vector to zero.
    if la.norm(B[:, 1]) > verySmallNumber :
        B[:, 1] = B[:, 1] / la.norm(B[:, 1])
    else :
        B[:, 1] = np.zeros_like(B[:, 1])
    # Now we need to repeat the process for column 2.
    # Insert two lines of code, the first to subtract the overlap with the zeroth vector,
    # and the second to subtract the overlap with the first.
    B[:, 2] = B[:, 2] - B[:, 2] @ B[:, 0] * B[:, 0]
    B[:, 2] = B[:, 2] - B[:, 2] @ B[:, 1] * B[:, 1]
    # Again we'll need to normalise our new vector.
    # Copy and adapt the normalisation fragment from above to column 2.
    if la.norm(B[:, 2]) > verySmallNumber :
        B[:, 2] = B[:, 2] / la.norm(B[:, 2])
    else :
        B[:, 2] = np.zeros_like(B[:, 2])
    # Finally, column three:
    # Insert code to subtract the overlap with the first three vectors.
    B[:, 3] = B[:, 3] - B[:, 3] @ B[:, 0] * B[:, 0]
    B[:, 3] = B[:, 3] - B[:, 3] @ B[:, 1] * B[:, 1]
    B[:, 3] = B[:, 3] - B[:, 3] @ B[:, 2] * B[:, 2]
    # Now normalise if possible
    if la.norm(B[:, 3]) > verySmallNumber :
        B[:, 3] = B[:, 3] / la.norm(B[:, 3])
    else :
        B[:, 3] = np.zeros_like(B[:, 3])
    # Finally, we return the result:
    return B

# The second part of this exercise will generalise the procedure.
# Previously, we could only have four vectors, and there was a lot of repeating in the code.
# We'll use a for-loop here to iterate the process for each vector.
def gsBasis(A) :
    B = np.array(A, dtype=np.float_) # Make B as a copy of A, since we're going to alter it's values.
    # Loop over all vectors, starting with zero, label them with i
    for i in range(B.shape[1]) :
        # Inside that loop, loop over all previous vectors, j, to subtract.
        for j in range(i) :
            # Complete the code to subtract the overlap with previous vectors.
            # you'll need the current vector B[:, i] and a previous vector B[:, j]
            B[:, i] = B[:, i] - B[:, i] @ B[:, j] * B[:, j]
        # Next insert code to do the normalisation test for B[:, i]
        if la.norm(B[:, i]) > verySmallNumber :
            B[:, i] = B[:, i] / la.norm(B[:, i])
        else :
            B[:, i] = np.zeros_like(B[:, i])
    # Finally, we return the result:
    return B

# This function uses the Gram-schmidt process to calculate the dimension
# spanned by a list of vectors.
# Since each vector is normalised to one, or is zero,
# the sum of all the norms will be the dimension.
def dimensions(A) :
    return np.sum(la.norm(gsBasis(A), axis=0))
```