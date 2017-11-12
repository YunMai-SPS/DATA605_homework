<<<<<<< HEAD
YMai\_Assignment2
================
Yun Mai
September 5, 2017

**1. Problem set 1**

1.  Show that *A*<sup>*T*</sup>*A* ≠ *A**A*<sup>*T*</sup> in general. (Proof and demonstration.)

2.  For a special type of square matrix A, we get *A*<sup>*T*</sup>*A* ≠ *A**A*<sup>*T*</sup>. Under what conditions could this be true? (Hint: The Identity matrix I is an example of such a matrix).

**Answer**

***Proof***

For a n x m matrix *A*, *A*<sup>*T*</sup> is a m x n matrix. *A*<sup>*T*</sup>*A* is m x m matrix. *A**A*<sup>*T*</sup> is a n x n matrix.

if *n* ≠ *m*, the size of *A*<sup>*T*</sup>*A* and *A**A*<sup>*T*</sup> are different, then we know *A*<sup>*T*</sup>*A* ≠ *A**A*<sup>*T*</sup>

Even if *n* = *m* and *A*<sup>*T*</sup>*A* and *A**A*<sup>*T*</sup> are the same size square matrices, *A*<sup>*T*</sup>*A* and *A**A*<sup>*T*</sup> are not equal at most of the time.

According to Theorem MMT Matrix Multiplication and Transposes, Suppose A is an m x n matrix and B is an n x m matrix. Then (AB)t=BtAt(AB)t=BtAt, we know that

So *A*<sup>*T*</sup>*A* is the transpose of itself.

assuming *A*<sup>*T*</sup>*A* = *A**A*<sup>*T*</sup>

(*A*<sup>*T*</sup>*A*)<sup>*T*</sup> = (*A*<sup>*T*</sup>)<sup>*T*</sup>*A*<sup>*T*</sup> is aganist Theorem MMT, so the assumption *A*<sup>*T*</sup>*A* = *A**A*<sup>*T*</sup> is not right, which means *A*<sup>*T*</sup>*A* ≠ *A**A*<sup>*T*</sup> in general.

For example,
``` r
(A <- matrix(c(1,2,3,4,5,6),2,byrow=T))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    4    5    6

``` r
(AAT <- A%*%t(A))
```

    ##      [,1] [,2]
    ## [1,]   14   32
    ## [2,]   32   77

``` r
(ATA <- t(A)%*%A)
```

    ##      [,1] [,2] [,3]
    ## [1,]   17   22   27
    ## [2,]   22   29   36
    ## [3,]   27   36   45

``` r
identical(AAT,ATA)
```

    ## [1] FALSE

``` r
(A <- matrix(c(1,2,3,4),2,byrow=T))
```

    ##      [,1] [,2]
    ## [1,]    1    2
    ## [2,]    3    4

``` r
(AAT <- A%*%t(A))
```

    ##      [,1] [,2]
    ## [1,]    5   11
    ## [2,]   11   25

``` r
(ATA <- t(A)%*%A)
```

    ##      [,1] [,2]
    ## [1,]   10   14
    ## [2,]   14   20

``` r
identical(AAT,ATA)
```

    ## [1] FALSE

**2. Problem set 2**

Matrix factorization is a very important problem. There are supercomputers built just to do matrix factorizations. Every second you are on an airplane, matrices are being factorized. Radars that track flights use a technique called Kalman ltering. At the heart of Kalman Filtering is a Matrix Factorization operation. Kalman Filters are solving linear systems of equations when they track your ight using radars.

Write an R function to factorize a square matrix A into LU or LDU, whichever you prefer. Please submit your response in an R Markdown document using our class naming convention, E.g. LFulton\_Assignment2\_PS2.png

You don't have to worry about permuting rows of A and you can assume that A is less than 5x5, if you need to hard-code any variables in your code. If you doing the entire assignment in R, then please submit only one markdown document for both the problems.

``` r
# According to the Doolittle algorithm for LU decomposition the diagonal is assumed to belong to the upper triangle. The diagonal for the lower triangle is then assumed to be all 1s.

# for a square matrix x, LU decomposition function could be written as following:
lu_decp_u <- function(x){
  # the number of rows 
  n <- dim(x)[1]
  # create empty matrices for L and U
  L <- matrix(rep(0,n^2), nrow = n, byrow=T)
  U <- matrix(rep(0,n^2), nrow = n, byrow=T)
  # the first column of lower triangel matrix L is the same as the first column of x
  L[,1] <- x[,1]
  # Also set the diaganal of L as 1
  for(i in 1:n){
    L[i,i] <- 1
  }
  # the first row of upper triangel matrix U is the same as the first row of x
  U[1,] <- x[1,]
  # calculate row by row starting from U[2,], then L[3,], U[3,],L[4,],U[4],...L[n,], and U[n,]
  for (i in 2:n){
    for(j in 2:n){
      # calculate the upper triangel matrix U first, column index is larger than row index
      if (j >= i){
        sum_u <- 0
        for(k in 1:n){
          if(k!=i){
            sum_u <- sum_u + L[i,k]*U[k,j]
          }
        }
        U[i,j]<-(x[i,j]-sum_u)/L[i,i]
      }
      # then calculate the lower  triangel matrix L, column index is smaller than row index
      else{
        sum_l <- 0
        for(k in 1:n){
          if(k!=j){
            sum_l <- sum_l +L[i,k]*U[k,j]
          }
        }
        L[i,j]<-(x[i,j]-sum_l)/U[j,j]
      }
    }
  }
  return(U)
} 

lu_decp_l <- function(x){
  # the number of rows 
  n <- dim(x)[1]
  # create empty matrices for L and U
  L <- matrix(rep(0,n*n),4,byrow=T)
  U <- matrix(rep(0,n*n),4,byrow=T)
  # the first column of lower  triangel matrix L is the same as the first column of x
  L[,1] <- x[,1]
  # Also set the diaganal of L as 1
  for(i in 1:n){
    L[i,i] <- 1
  }
  # the first row of upper  triangel matrix U is the same as the first row of x
  U[1,] <- x[1,]
  # calculate row by row starting from U[2,], then L[3,], U[3,],L[4,],U[4],...L[n,], and U[n,]
  for (i in 2:n){
    for(j in 2:n){
      # calculate the upper  triangel matrix U first, column index is larger than row index
      if (j >= i){
        sum_u <- 0
        for(k in 1:n){
          if(k!=i){
            sum_u <- sum_u + L[i,k]*U[k,j]
          }
        }
        U[i,j]<-(x[i,j]-sum_u)/L[i,i]
      }
      # then calculate the lower  triangel matrix L, column index is smaller than row index
      else{
        sum_l <- 0
        for(k in 1:n){
          if(k!=j){
            sum_l <- sum_l +L[i,k]*U[k,j]
          }
        }
        L[i,j]<-(x[i,j]-sum_l)/U[j,j]
      }
    }
  }
  return(L)
} 

(B <- matrix(c(1,2,3,4,2,1,1,2,4,2,8,6,2,4,2,8),4,byrow=T))
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    2    1    1    2
    ## [3,]    4    2    8    6
    ## [4,]    2    4    2    8

``` r
(l<- lu_decp_l(B))
```

    ##      [,1] [,2]       [,3] [,4]
    ## [1,]    1    0  0.0000000    0
    ## [2,]    2    1  0.0000000    0
    ## [3,]    4    2  1.0000000    0
    ## [4,]    2    0 -0.6666667    1

``` r
(u<- lu_decp_u(B))
```

    ##      [,1] [,2] [,3]      [,4]
    ## [1,]    1    2    3  4.000000
    ## [2,]    0   -3   -5 -6.000000
    ## [3,]    0    0    6  2.000000
    ## [4,]    0    0    0  1.333333

``` r
(lu_m <-l%*%u)
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    2    1    1    2
    ## [3,]    4    2    8    6
    ## [4,]    2    4    2    8

Then, use lu and expand functions from Matrix library to do decomposition.

``` r
library(Matrix)
lum_b <- lu(B)
elu <- expand(lum_b)
(lu_b <- elu$P%*%elu$L%*%elu$U)
```

    ## 4 x 4 Matrix of class "dgeMatrix"
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    2    1    1    2
    ## [3,]    4    2    8    6
    ## [4,]    2    4    2    8

``` r
all(lu_b,lu_m) # lu_m is the product of mannually built function and lu_b is the package built_in function.
```

    ## Warning in all(x@x, ..., na.rm = na.rm): coercing argument of type 'double'
    ## to logical

    ## Warning in all(x@x, ..., na.rm = na.rm): coercing argument of type 'double'
    ## to logical

    ## [1] TRUE

    The L and U calcualted by two methods are differernt because lu function decomposed the matrix as P,L,and U.
=======
YMai\_Assignment2
================
Yun Mai
September 5, 2017

**1. Problem set 1**

1.  Show that *A*<sup>*T*</sup>*A* ≠ *A**A*<sup>*T*</sup> in general. (Proof and demonstration.)

2.  For a special type of square matrix A, we get *A*<sup>*T*</sup>*A* ≠ *A**A*<sup>*T*</sup>. Under what conditions could this be true? (Hint: The Identity matrix I is an example of such a matrix).

**Answer**

***Proof***

For a n x m matrix *A*, *A*<sup>*T*</sup> is a m x n matrix. *A*<sup>*T*</sup>*A* is m x m matrix. *A**A*<sup>*T*</sup> is a n x n matrix.

if *n* ≠ *m*, the size of *A*<sup>*T*</sup>*A* and *A**A*<sup>*T*</sup> are different, then we know *A*<sup>*T*</sup>*A* ≠ *A**A*<sup>*T*</sup>

Even if *n* = *m* and *A*<sup>*T*</sup>*A* and *A**A*<sup>*T*</sup> are the same size square matrices, *A*<sup>*T*</sup>*A* and *A**A*<sup>*T*</sup> are not equal at most of the time.

According to Theorem MMT Matrix Multiplication and Transposes, Suppose A is an m x n matrix and B is an n x m matrix. Then (AB)t=BtAt(AB)t=BtAt, we know that

So *A*<sup>*T*</sup>*A* is the transpose of itself.

assuming *A*<sup>*T*</sup>*A* = *A**A*<sup>*T*</sup>

(*A*<sup>*T*</sup>*A*)<sup>*T*</sup> = (*A*<sup>*T*</sup>)<sup>*T*</sup>*A*<sup>*T*</sup> is aganist Theorem MMT, so the assumption *A*<sup>*T*</sup>*A* = *A**A*<sup>*T*</sup> is not right, which means *A*<sup>*T*</sup>*A* ≠ *A**A*<sup>*T*</sup> in general.

For example,
``` r
(A <- matrix(c(1,2,3,4,5,6),2,byrow=T))
```

    ##      [,1] [,2] [,3]
    ## [1,]    1    2    3
    ## [2,]    4    5    6

``` r
(AAT <- A%*%t(A))
```

    ##      [,1] [,2]
    ## [1,]   14   32
    ## [2,]   32   77

``` r
(ATA <- t(A)%*%A)
```

    ##      [,1] [,2] [,3]
    ## [1,]   17   22   27
    ## [2,]   22   29   36
    ## [3,]   27   36   45

``` r
identical(AAT,ATA)
```

    ## [1] FALSE

``` r
(A <- matrix(c(1,2,3,4),2,byrow=T))
```

    ##      [,1] [,2]
    ## [1,]    1    2
    ## [2,]    3    4

``` r
(AAT <- A%*%t(A))
```

    ##      [,1] [,2]
    ## [1,]    5   11
    ## [2,]   11   25

``` r
(ATA <- t(A)%*%A)
```

    ##      [,1] [,2]
    ## [1,]   10   14
    ## [2,]   14   20

``` r
identical(AAT,ATA)
```

    ## [1] FALSE

**2. Problem set 2**

Matrix factorization is a very important problem. There are supercomputers built just to do matrix factorizations. Every second you are on an airplane, matrices are being factorized. Radars that track flights use a technique called Kalman ltering. At the heart of Kalman Filtering is a Matrix Factorization operation. Kalman Filters are solving linear systems of equations when they track your ight using radars.

Write an R function to factorize a square matrix A into LU or LDU, whichever you prefer. Please submit your response in an R Markdown document using our class naming convention, E.g. LFulton\_Assignment2\_PS2.png

You don't have to worry about permuting rows of A and you can assume that A is less than 5x5, if you need to hard-code any variables in your code. If you doing the entire assignment in R, then please submit only one markdown document for both the problems.

``` r
# According to the Doolittle algorithm for LU decomposition the diagonal is assumed to belong to the upper triangle. The diagonal for the lower triangle is then assumed to be all 1s.

# for a square matrix x, LU decomposition function could be written as following:
lu_decp_u <- function(x){
  # the number of rows 
  n <- dim(x)[1]
  # create empty matrices for L and U
  L <- matrix(rep(0,n^2), nrow = n, byrow=T)
  U <- matrix(rep(0,n^2), nrow = n, byrow=T)
  # the first column of lower triangel matrix L is the same as the first column of x
  L[,1] <- x[,1]
  # Also set the diaganal of L as 1
  for(i in 1:n){
    L[i,i] <- 1
  }
  # the first row of upper triangel matrix U is the same as the first row of x
  U[1,] <- x[1,]
  # calculate row by row starting from U[2,], then L[3,], U[3,],L[4,],U[4],...L[n,], and U[n,]
  for (i in 2:n){
    for(j in 2:n){
      # calculate the upper triangel matrix U first, column index is larger than row index
      if (j >= i){
        sum_u <- 0
        for(k in 1:n){
          if(k!=i){
            sum_u <- sum_u + L[i,k]*U[k,j]
          }
        }
        U[i,j]<-(x[i,j]-sum_u)/L[i,i]
      }
      # then calculate the lower  triangel matrix L, column index is smaller than row index
      else{
        sum_l <- 0
        for(k in 1:n){
          if(k!=j){
            sum_l <- sum_l +L[i,k]*U[k,j]
          }
        }
        L[i,j]<-(x[i,j]-sum_l)/U[j,j]
      }
    }
  }
  return(U)
} 

lu_decp_l <- function(x){
  # the number of rows 
  n <- dim(x)[1]
  # create empty matrices for L and U
  L <- matrix(rep(0,n*n),4,byrow=T)
  U <- matrix(rep(0,n*n),4,byrow=T)
  # the first column of lower  triangel matrix L is the same as the first column of x
  L[,1] <- x[,1]
  # Also set the diaganal of L as 1
  for(i in 1:n){
    L[i,i] <- 1
  }
  # the first row of upper  triangel matrix U is the same as the first row of x
  U[1,] <- x[1,]
  # calculate row by row starting from U[2,], then L[3,], U[3,],L[4,],U[4],...L[n,], and U[n,]
  for (i in 2:n){
    for(j in 2:n){
      # calculate the upper  triangel matrix U first, column index is larger than row index
      if (j >= i){
        sum_u <- 0
        for(k in 1:n){
          if(k!=i){
            sum_u <- sum_u + L[i,k]*U[k,j]
          }
        }
        U[i,j]<-(x[i,j]-sum_u)/L[i,i]
      }
      # then calculate the lower  triangel matrix L, column index is smaller than row index
      else{
        sum_l <- 0
        for(k in 1:n){
          if(k!=j){
            sum_l <- sum_l +L[i,k]*U[k,j]
          }
        }
        L[i,j]<-(x[i,j]-sum_l)/U[j,j]
      }
    }
  }
  return(L)
} 

(B <- matrix(c(1,2,3,4,2,1,1,2,4,2,8,6,2,4,2,8),4,byrow=T))
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    2    1    1    2
    ## [3,]    4    2    8    6
    ## [4,]    2    4    2    8

``` r
(l<- lu_decp_l(B))
```

    ##      [,1] [,2]       [,3] [,4]
    ## [1,]    1    0  0.0000000    0
    ## [2,]    2    1  0.0000000    0
    ## [3,]    4    2  1.0000000    0
    ## [4,]    2    0 -0.6666667    1

``` r
(u<- lu_decp_u(B))
```

    ##      [,1] [,2] [,3]      [,4]
    ## [1,]    1    2    3  4.000000
    ## [2,]    0   -3   -5 -6.000000
    ## [3,]    0    0    6  2.000000
    ## [4,]    0    0    0  1.333333

``` r
(lu_m <-l%*%u)
```

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    2    1    1    2
    ## [3,]    4    2    8    6
    ## [4,]    2    4    2    8

Then, use lu and expand functions from Matrix library to do decomposition.

``` r
library(Matrix)
lum_b <- lu(B)
elu <- expand(lum_b)
(lu_b <- elu$P%*%elu$L%*%elu$U)
```

    ## 4 x 4 Matrix of class "dgeMatrix"
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    2    3    4
    ## [2,]    2    1    1    2
    ## [3,]    4    2    8    6
    ## [4,]    2    4    2    8

``` r
all(lu_b,lu_m) # lu_m is the product of mannually built function and lu_b is the package built_in function.
```

    ## Warning in all(x@x, ..., na.rm = na.rm): coercing argument of type 'double'
    ## to logical

    ## Warning in all(x@x, ..., na.rm = na.rm): coercing argument of type 'double'
    ## to logical

    ## [1] TRUE

    The L and U calcualted by two methods are differernt because lu function decomposed the matrix as P,L,and U.
>>>>>>> d712b566f3ac68e1078dba63ffea496aa72d286c
