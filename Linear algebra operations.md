
# Vectors
## Vector size
$$|x| = \sqrt(sum(x^2))$$
```R
size <- function(x) {
	return(sqrt(sum(x^2)))
}
```

```python
def size(x):
	return sum(x**2)**(0.5)
```
## Dot product
$$x · y = sum(x*y)$$
```R
x %*% y
```

```python
def dot(x, y):
	return sum(x*y)

numpy.dot(x, y)
```
## Projection of vector *s* on vector *r*
$$s' = r × \frac{r · s}{|r|^2} $$
$$s' = r × \frac{r · s}{r · r}$$
```R
projection <- function(s, r) {
	p <- (r %*% s)/(r %*% r) * r
	return(p)
}
```

### Size of the projection of *s* on *r*
$$|s'| = \frac{r · s}{|r|}$$
```R
projection_size <- function(s, r) {
	ps <- (r %*% s) / sqrt(sum(r^2))
	return(ps)
}
```

## Change basis of vector *r* to orthogonal coord sistem *b1* and *b2*
$$i = \frac{r · b1}{|b1|^2}$$
$$j = \frac{r · b2}{|b2|^2}$$
$$r' = \begin{array}{} a \\ b \end{array}$$
```R
chbase <- function(r, b1, b2) {
	i <- (r %*% b1)/(sqrt(sum(b1))^2)
	j <- (r %*% b2)/(sqrt(sum(b2))^2)
	return(c(i, j))
}
```

# Matrices

## Transform a vector *r*  to *r'* through a matrix *A*
Given a vector $r$, we can change the basis to the coordinate system in the matrix $A$ (e.g. $\begin{array}{} 1 & 0 \\ 0 & 1 \end{array}$) by calculating the product $Ar$.
$$A = \begin{array}{cc} a &c \\ b &d \end{array} $$
$$r = \begin{array}{} i \\ j \end{array}$$
$$Ar = \begin{array}{} ai+cj \\ bi+dj \end{array} = \begin{array}{} r · [a,b] \\ r · [c,d] \end{array}$$
```R
A %*% r
```
```python
numpy.dot(A, r)
```
## Transform a matrix *A* to *B* through a matrix *M*
$$A = \begin{array}{cc} a &c \\ b &d \end{array} $$
$$M = \begin{array}{cc} i &k \\ j &l \end{array} $$
$$B = \begin{array}{} [a,b] · [i,j] & [a,b] · [k,l] \\ [c,d] · [i,j] & [c,d] · [k,l] \end{array}$$
```R
A %*% M
```
```python
numpy.dot(A, M)
```
## Matrix inversion
The product of a matrix and its inverse is the identity matrix: $A A^{-1} = I$ or $A^{-1} A = I$.
To find the inverse of $A$, we can transform $A$ to $I$ in the above equation, using eliminations:
$$A^{-1}A = I$$
$$A^{-1}I = M$$
$$A^{-1} = M$$
```python
numpy.linalg.inv(A)
```
### Non invertible matrices
A matrix is not invertible if it has determinant equal to zero.
To check a matrix determinant:
```python
numpy.linalg.det(A)
```
