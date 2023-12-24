
# Vectors
## Vector size
$$|x| = \sqrt(sum(x^2))$$
```R
size <- function(x) {
	return(sqrt(sum(x^2)))
}
```

## Dot product
$$x · y = sum(x*y)$$
```R
x %*% y
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
$$A = \begin{array}{cc} a &c \\ b &d \end{array} $$
$$r = \begin{array}{} i \\ j \end{array}$$
$$Ar = \begin{array}{} ai+bj \\ ci+dj \end{array}$$
