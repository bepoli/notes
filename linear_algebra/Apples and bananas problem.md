# The problem
On day *a* I buy 2 apples and 3 bananas for 8€. On day *b*, I buy 10 apples and 1 banana for 13€. What's the price of apples and bananas?

# Solve with linear algebra
$$(\begin{array}{} 2 &3 \\ 10 &1 \end{array})(\begin{array}{} a \\ b \end{array}) = (\begin{array}{} 8 \\ 13 \end{array})$$
$$Ar = s$$
I want to know what $r$ is.
## Using eliminations, taking rows out of each other
$$(\begin{array}{} 1 &1 &3 \\ 1 &2 &4 \\ 1 &1 &2 \end{array}) (\begin{array}{} a \\ b \\ c \end{array}) = (\begin{array}{} 15 \\ 21 \\ 13 \end{array})$$
Take $a$ out of $b$ and $c$:
$$(\begin{array}{} 1 &1 &3 \\ 0 &1 &1 \\ 0 &0 &1 \end{array}) (\begin{array}{} a \\ b \\ c \end{array}) = (\begin{array}{} 15 \\ 6 \\ 2 \end{array})$$
Take $c$ out of $a$ and $b$:
$$(\begin{array}{} 1 &1 &0 \\ 0 &1 &0 \\ 0 &0 &1 \end{array}) (\begin{array}{} a \\ b \\ c \end{array}) = (\begin{array}{} 9 \\ 4 \\ 2 \end{array})$$
Take $b$ out of $a$:
$$(\begin{array}{} 1 &0 &0 \\ 0 &1 &0 \\ 0 &0 &1 \end{array}) (\begin{array}{} a \\ b \\ c \end{array}) = (\begin{array}{} 5 \\ 4 \\ 2 \end{array})$$
The $A$ matrix has become the identity matrix $I$. Since $Ir = r$, then:
$$\begin{array}{} a \\ b \\ c \end{array} = \begin{array}{} 5 \\ 4 \\ 2 \end{array}$$
## Using matrix inversion

Find $r$ by calculating the inverse of $A$.
$$Ar = s$$
Multiply both ends by the inverse of $A$:
$$A^{-1}Ar = A^{-1}s$$
Since $A{^-1}A = I$:
$$Ir = A{-1}s$$
$$r = A^{-1}s$$
So, to solve the problem I need to calculate the inverse of $A$.

## Find the inverse of a matrix by eliminations
Let $Ar = s$ be:
$$(\begin{array}{} 1 &1 &3 \\ 1 &2 &4 \\ 1 &1 &2 \end{array}) (\begin{array}{} a \\ b \\ c \end{array}) = (\begin{array}{} 15 \\ 21 \\ 13 \end{array})$$
Let's find the inverse of $A$ by transforming $A$ in $I$.
$$AA^{-1} = I$$
$$(\begin{array}{} 1 &1 &3 \\ 1 &2 &4 \\ 1 &1 &2 \end{array}) A^{-1} = (\begin{array}{} 1 &0 &0 \\ 0 &1 &0 \\ 0 &0 &1 \end{array})$$
First, let's get all zeros on the bottom left side of the diagonal (Echelon form).
Substract first row from third and second:
$$(\begin{array}{} 1 &1 &3 \\ 0 &1 &1 \\ 0 &0 &1 \end{array}) A^{-1} = (\begin{array}{} 1 &0 &0 \\ -1 &1 &0 \\ 1 &0 &-1 \end{array})$$
Take the third row out of the second and 3x out of the first_
$$(\begin{array}{} 1 &1 &0 \\ 0 &1 &0 \\ 0 &0 &1 \end{array}) A^{-1} = (\begin{array}{} -2 &0 &3 \\ -2 &1 &1 \\ 1 &0 &-1 \end{array})$$
Take the second row out of the first:
$$(\begin{array}{} 1 &0 &0 \\ 0 &1 &0 \\ 0 &0 &1 \end{array}) A^{-1} = (\begin{array}{} 0 &-1 &2 \\ -2 &1 &1 \\ 1 &0 &-1 \end{array})$$
That is:
$$IA^{-1} = (\begin{array}{} 0 &-1 &2 \\ -2 &1 &1 \\ 1 &0 &-1 \end{array})$$
$$A^{-1} = (\begin{array}{} 0 &-1 &2 \\ -2 &1 &1 \\ 1 &0 &-1 \end{array})$$
