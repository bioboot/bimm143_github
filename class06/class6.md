# Class06: R Functions
Barry (PID: 911)

- [Background](#background)
- [A first function](#a-first-function)
- [A second function](#a-second-function)
- [A new cool function](#a-new-cool-function)

## Background

Functions are at the heart of using R. Everything we do involves calling
and using functions (from data input, analysis to results output).

All functions in R have at least 3 things:

1.  A **name** the thing we use to call the function.
2.  One or more input **arguments** that a are comma separated
3.  The **body**, lines of code between curly brackets { } that does the
    work of the function.

## A first function

Let’s write a silly wee function to add some numbers:

``` r
add <- function(x) {
  x + 1
}
```

Let’s try it out

``` r
add(100)
```

    [1] 101

Will this work

``` r
add( c(100, 200, 300) )
```

    [1] 101 201 301

Modify to be more useful and add more than just 1

``` r
add <- function(x, y=1) {
  x + y
}
```

``` r
add(100, 10)
```

    [1] 110

Will this work?

``` r
add(100)
```

    [1] 101

``` r
plot(1:10, col="blue", typ="b")
```

![](class6_files/figure-commonmark/unnamed-chunk-7-1.png)

``` r
log(10, base=10)
```

    [1] 1

> **N.B.** Input arguments can be either **required** or **optional**.
> The later have a fall-back default that is specifed in the function
> code with an equals sign.

``` r
#add(x=100, y=200, z=300)
```

## A second function

All functions in R look like this

``` r
name <- function(arg) { 
  body 
}
```

The `sample()` function in R …

``` r
sample(1:10, size = 4)
```

    [1]  6 10  1  8

> Q. Return 12 numbers picked randomly from the input 1:10

``` r
sample(1:10, size=12, replace = TRUE)
```

     [1]  1  4  4  4  6  1  1  7 10  2  2  3

> Q. Write the code to generate a random 12 nucleotide long DNA
> sequence?

``` r
bases <- c("A","C","G","T")
sample(bases, size=12, replace=TRUE)
```

     [1] "G" "T" "C" "C" "A" "C" "G" "A" "T" "A" "C" "T"

> Q. Wite a first version function called `generate_dna()` that
> generates a user specified length `n` random DNA sequence?

    name <- function(arg) { 
      body 
    }

``` r
generate_dna <- function(n=6) {
  bases <- c("A","C","G","T")
  sample(bases, size=n, replace=TRUE)
}
```

``` r
generate_dna(100)
```

      [1] "T" "G" "A" "G" "C" "T" "C" "A" "T" "T" "C" "T" "G" "G" "G" "C" "A" "T"
     [19] "T" "T" "C" "T" "T" "C" "T" "G" "C" "A" "G" "C" "A" "C" "G" "G" "G" "T"
     [37] "G" "C" "G" "G" "G" "T" "G" "C" "T" "G" "T" "A" "G" "T" "A" "G" "A" "A"
     [55] "C" "T" "G" "T" "G" "T" "C" "T" "A" "C" "G" "C" "T" "A" "G" "A" "G" "T"
     [73] "T" "T" "C" "G" "C" "A" "A" "C" "G" "C" "G" "T" "C" "A" "A" "A" "T" "G"
     [91] "A" "A" "T" "A" "A" "A" "A" "A" "C" "A"

> Q. Modify your function to return a FASTA like sequence so rather than
> \[1\] “G” “C” “A” “A” “T” we want “GCAAT”

``` r
generate_dna <- function(n=6) {
  bases <- c("A","C","G","T")
  ans <- sample(bases, size=n, replace=TRUE)
  ans <- paste(ans, collapse = "")
  return(ans)
  x <-"poopoopants"
  x
}
```

``` r
generate_dna(10)
```

    [1] "GCTAGCTTAC"

An example

``` r
# Example pattern (not using your bases)
x <- c("H", "E", "L", "L", "O")

paste(x, collapse = "****")
```

    [1] "H****E****L****L****O"

``` r
# returns "HELLO"
```

> Q. Give the user an option to return FASTA foramt output sequence or
> standard multi-element vector format?

``` r
generate_dna <- function(n=6, fasta=TRUE) {
  bases <- c("A","C","G","T")
  ans <- sample(bases, size=n, replace=TRUE)
  
  if(fasta) {  
    ans <- paste(ans, collapse = "")
    cat("Hello...")
  } else {
    cat("...is it me you are looking for...")
  }
  
  return(ans)
}
```

``` r
generate_dna(10)
```

    Hello...

    [1] "CAATCGCATA"

``` r
generate_dna(10, fasta=F)
```

    ...is it me you are looking for...

     [1] "T" "C" "A" "A" "A" "G" "C" "A" "A" "T"

## A new cool function

> Q. Write a function called `generate_protein()` that generates a user
> specifed length protein sequence in FASTA like format?

``` r
generate_protein <- function(n) {

  aa <- c( "A", "R", "N", "D", "C",
           "Q", "E", "G", "H", "I",
           "L", "K", "M", "F", "P",
           "S", "T", "W", "Y", "V")
  
  ans <- sample(aa, size=n, replace = T)
  ans <- paste(ans, collapse = "")
  return( ans )
} 
```

``` r
generate_protein(10)
```

    [1] "RTCEFITFRQ"

> Q. Use your new `generate_protein()` function to generate sequences
> between length 6 and 12 amino-acids in length and check of any of
> these are unique in nature (i.e. found in the NR database at NCBI)?

``` r
generate_protein(6)
```

    [1] "GHEPRN"

``` r
generate_protein(7)
```

    [1] "IVNGEQS"

``` r
generate_protein(8)
```

    [1] "SHHHAVEE"

``` r
generate_protein(9)
```

    [1] "HEWNRNMMI"

``` r
generate_protein(10)
```

    [1] "WYNGCLFPGL"

``` r
generate_protein(11)
```

    [1] "NNYLLGCNDVV"

``` r
generate_protein(12)
```

    [1] "QNWHAHNVDQPM"

Or we could do a `for()` loop:

``` r
for(i in 6:12) {
  cat(">", i, sep="", "\n")
  cat( generate_protein(i), "\n" )
}
```

    >6
    WLQTCG 
    >7
    NRCRVLN 
    >8
    FIDGREGI 
    >9
    WDMSLSQWI 
    >10
    AYPNCSHTLV 
    >11
    KLAEVWWLMDD 
    >12
    PCNGVNMKEVCE 

> id AGKRTST next GKRTTST
