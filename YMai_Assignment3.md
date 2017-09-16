YMai\_Assigment3\_605
================
Yun Mai
September 16, 2017

**Load the packages**

``` r
suppressWarnings(suppressMessages(library(Matrix))) # eigen and rankMatrix functions will be used 
suppressWarnings(suppressMessages(library(pracma))) #charpoly function will be used
```

1. Problem set 1
----------------

1.  What is the rank of the matrix A?

``` r
(A <- matrix(c(1, 2, 3, 4, 1, 0, 1, 3, 0, 1, 2, 1, 5, 4, 2, 3),4,byrow = T))
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    1    0    1    3
    ## [3,]    0    1    2    1
    ## [4,]    5    4    2    3

``` r
rank_a <- rankMatrix(A)
print(paste("rank of matrix A is ",rank_a))
```

    ## [1] "rank of matrix A is  4"

1.  Given an mxn matrix where m &gt; n, what can be the maximum rank? The minimum rank, assuming that the matrix is non-zero?

For an mxn matrix where m &gt; n, let B be a row-equivalent matrix in reduced row-echelon form. According to Theorem CRN(Computing Rank and Nullity), Let r denote the number of pivot columns (or the number of nonzero rows),then r(A) = r. For matrix B, the left most nonzero entry of each nonzero row is equal to 1 and it is the only nonzero entry in its column. The pivot columns could form a indentity matrix if we rearrange matrix B. If this indentity matrix is in size kxk, then it is obvious that the maximum of k is equal to m or n which ever is smaller. Here m &gt; n, so we know r &lt; n. For nonzero matrix, the minimum number of pivot columns would be 1. So r &gt;= 1. The minimum rank could be 1.

1.  What is the rank of matrix B?

``` r
(B <- matrix(c(1,2,1,3,6,3,2,4,2),3,byrow = T))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    1
    ## [2,]    3    6    3
    ## [3,]    2    4    2

``` r
rank_b <- rankMatrix(B)
print(paste("rank of matrix A is ",rank_b))
```

    ## [1] "rank of matrix A is  1"

2. Problem set 2
----------------

Compute the eigenvalues and eigenvectors of the matrix A. You'll need to show your work. You'll need to write out the characteristic polynomial and show your solution.

### eigenvalues

``` r
(C <- matrix(c(1,2,3,0,4,5,0,0,6),3,byrow = T))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    0    4    5
    ## [3,]    0    0    6

``` r
eigen_c <- eigen(C)
eigenvalue <- eigen_c$values
print(paste("eigenvalues includes:",list(round(eigenvalue))))
```

    ## [1] "eigenvalues includes: c(6, 4, 1)"

### eigenvectors

``` r
I3 <- matrix(c(1,0,0,0,1,0,0,0,1),3, byrow=T)
(NS_6 <- nullspace(C-6*I3)) # eigenvectors for # eigenvalue = 6
```

    ##           [,1]
    ## [1,] 0.5108407
    ## [2,] 0.7981886
    ## [3,] 0.3192754

``` r
(NS_4 <- nullspace(C-4*I3)) # eigenvectors for # eigenvalue = 4
```

    ##              [,1]
    ## [1,] 5.547002e-01
    ## [2,] 8.320503e-01
    ## [3,] 5.551115e-17

``` r
(NS_1 <- round(nullspace(C-1*I3))) # eigenvectors for # eigenvalue = 1
```

    ##      [,1]
    ## [1,]   -1
    ## [2,]    0
    ## [3,]    0

``` r
# or eigenvectors could be calculated as follows:
(reduce_6 <- rref(C-6*I3))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    0 -1.6
    ## [2,]    0    1 -2.5
    ## [3,]    0    0  0.0

``` r
(NS_six <- matrix(c(1.6,2.5,1),3,byrow=T)) # eigenvectors for # eigenvalue = 6
```

    ##      [,1]
    ## [1,]  1.6
    ## [2,]  2.5
    ## [3,]  1.0

``` r
(reduce_4 <- rref(C-4*I3))
```

    ##      [,1]       [,2] [,3]
    ## [1,]    1 -0.6666667    0
    ## [2,]    0  0.0000000    1
    ## [3,]    0  0.0000000    0

``` r
(NS_four <- matrix(c(0.67,1,0),3,byrow=T)) # eigenvectors for # eigenvalue = 4
```

    ##      [,1]
    ## [1,] 0.67
    ## [2,] 1.00
    ## [3,] 0.00

``` r
(reduce_1 <- rref(C-1*I3))
```

    ##      [,1] [,2] [,3]
    ## [1,]    0    1    0
    ## [2,]    0    0    1
    ## [3,]    0    0    0

``` r
(NS_one <- matrix(c(1,0,0),3,byrow=T)) # eigenvectors for # eigenvalue = 1
```

    ##      [,1]
    ## [1,]    1
    ## [2,]    0
    ## [3,]    0

### Characteristic Polynomials

``` r
(polyn <- charpoly(C,info=T))
```

    ## Error term: 0

    ## $cp
    ## [1]   1 -11  34 -24
    ## 
    ## $det
    ## [1] 24
    ## 
    ## $inv
    ##      [,1]  [,2]        [,3]
    ## [1,]    1 -0.50 -0.08333333
    ## [2,]    0  0.25 -0.20833333
    ## [3,]    0  0.00  0.16666667

So the Characteristic Polynomials is : *x*<sup>3</sup> − 11*x*<sup>2</sup> + 34*x* + 24 = (*x* − 6)(*x* − 4)(*x* − 1)

### The solution of the Characteristic Polynomial

``` r
print(paste("The solution of the Characteristic Polynomial are: ",list(roots(polyn$cp))))
```

    ## [1] "The solution of the Characteristic Polynomial are:  c(6, 4, 1)"
