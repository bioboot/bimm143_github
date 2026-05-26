# Class 6: R functions
Barry (PID: 911)

- [Background](#background)
- [A first function](#a-first-function)
- [A generate_dna() function](#a-generate_dna-function)
- [Write a `generate_protein()`
  function](#write-a-generate_protein-function)
- [Generate random protein sequences of length 6 to
  13](#generate-random-protein-sequences-of-length-6-to-13)
- [Are Our peptides “unique in
  nature”?](#are-our-peptides-unique-in-nature)
- [Connecting our findings to
  immunology](#connecting-our-findings-to-immunology)

## Background

All functions in R have at least 3 things:

- a *name* (we pick that and use it to call the function)
- input *arguments* (one or more comma separated inputs that go inside
  the brackets when we call the function),
- the *body* (the lines of R code that do the work of the function).

## A first function

Here we will create a function to add some numbers. Let’s call it
`add()`.

Input arguments can be either **“required”** or **“optional”**. The
later have fall-back default values that will be used if the user does
not specify them.

> **Q1a**. Your first version of the function should add two input
> numbers together. For example, add(4, 7) should return 11. \[1 pt\]

``` r
add <- function(x, y=0) {
  x + y
}
```

Can we use our new function:

``` r
add(10, 100)
```

    [1] 110

``` r
add(10)
```

    [1] 10

> **Q1b**: For you second version, adapt your first function so it can
> take a single input vector or two inputs as before. For example,
> add(4, 7) and add( c(4,7,10) ). \[1 pt\]

``` r
add <- function(x, y=0) {
  sum(x,y)
}
```

``` r
add(4, 7)
```

    [1] 11

``` r
add( c(4,7,10))
```

    [1] 21

> **Q1c**. To do on your own… create a third version of your function
> that can add any number of inputs that the user provides. For example,
> add(1, 2, 3, -4)

``` r
add <- function(x, y=0, z=0) {
  sum(x,y,z)
}
```

``` r
add(x=1, y=2, z=13)
```

    [1] 16

``` r
ans <- add(1, 2, 3)
ans
```

    [1] 6

We can explicitly set a **return** value output from a function (rather
than the default last line) by using the `return()` function call.

``` r
add <- function(x, y=0, z=0) {
  return(sum(x,y,z))
  cat("Is it break time yet?\n")
}

add(10,100)
```

    [1] 110

## A generate_dna() function

A useful function here is the “base R” `sample()` function:

``` r
sample(1:5, size=3)
```

    [1] 2 1 3

``` r
sample(1:5, size=60, replace = TRUE)
```

     [1] 2 3 1 4 2 3 5 2 5 1 2 1 3 3 2 4 4 5 1 4 3 2 1 5 3 1 5 2 4 5 5 1 3 5 3 3 4 4
    [39] 3 3 5 1 1 2 1 3 5 1 3 4 5 4 1 1 1 2 2 2 4 4

We can use this to make a random nucleotide sequence if we draw from
“A”, “C”, “G” and “T”…

``` r
sample(x=c("A","C","G","T"), size=10, replace = TRUE)
```

     [1] "G" "T" "G" "A" "G" "A" "C" "C" "C" "G"

> **Q2a**: Write a function `generate_dna()` that returns a random DNA
> sequence of a length specified by the user. Your first version should
> return a multi-element vector of single character nucleotides. For
> example generate_dna(6) might return “A”, “T”, “T”, “G”, “A”, “C”. \[1
> pt\]

``` r
generate_dna <- function(len=10) {
  sample(x=c("A","C","G","T"), size=len, replace = TRUE)
}
```

``` r
generate_dna()
```

     [1] "G" "A" "A" "A" "A" "A" "C" "A" "A" "T"

``` r
generate_dna(len=100)
```

      [1] "C" "T" "T" "A" "T" "C" "G" "T" "T" "G" "T" "T" "C" "C" "T" "T" "A" "G"
     [19] "C" "T" "T" "G" "T" "A" "A" "G" "G" "T" "T" "C" "G" "A" "A" "C" "C" "T"
     [37] "C" "G" "C" "A" "T" "A" "A" "T" "A" "T" "T" "T" "A" "A" "A" "C" "T" "T"
     [55] "C" "C" "G" "T" "T" "C" "T" "C" "C" "G" "T" "T" "G" "A" "T" "T" "C" "C"
     [73] "G" "T" "G" "A" "C" "C" "G" "C" "T" "C" "C" "A" "A" "G" "C" "G" "C" "G"
     [91] "A" "A" "G" "T" "A" "A" "A" "C" "G" "G"

> **Q2b**: Your second version should *optionally* be able to return
> either a multi-element vector of single character nucleotides (as
> before) or a **single character string** (not a vector of single
> letters but a singe vector of multiple letters). For example
> “AAGGCTG”. \[1 pt\]

``` r
generate_dna <- function(len=10, single.element=TRUE) {

  ans <- sample(x=c("A","C","G","T"), size=len, replace = TRUE)

  #cat("Hello....")
  
  if(single.element) {
    # cat("is it me you are looking for...")
    ans <- paste( ans, collapse = "" )
  }
  return(ans)
}
```

Functions that could be useful here are `paste()`, `if()`, `cat()` and
`return()`

``` r
generate_dna()
```

    [1] "GCCATCTTGC"

``` r
generate_dna(single.element = FALSE)
```

     [1] "A" "C" "A" "T" "T" "T" "C" "A" "T" "G"

> **Q2c**. Finally, create a final version of your function that prints
> out a FASTA format sequence with an id line indicating the sequence
> length.

    >len9
    CGAAGGCTG

``` r
cat("hello \n there")
```

    hello 
     there

``` r
generate_dna <- function(len=10, single.element=TRUE) {

  ans <- sample(x=c("A","C","G","T"), size=len, replace = TRUE)

  if(single.element) {
     ans <- paste( ans, collapse = "" )
  }
  
  ## Format as FASTA with an ID line
  cat( paste(">len", len, "\n", sep="") )
  cat(ans)
  cat("\n")
  ## 
  
  return(ans)
}
```

``` r
x <- generate_dna(20)
```

    >len20
    CCCCCTTGCGTACGCAGCTA

## Write a `generate_protein()` function

> **Q3**. Write a function `generate_protein()` that returns a random
> peptide/protein sequence of a length specified by the user. For
> example `generate_protein(6)` might return `"WQRTAG"`.

``` r
generate_protein <- function(len=9) {
  aa <- c("A", "R", "N", "D", "C", "E", "Q", 
          "G", "H", "I", "L", "K", "M", "F", 
          "P", "S","T", "W", "Y", "V")
  
  ans <- sample(x=aa, size=len, replace = TRUE)
  paste(ans, collapse = "")
}
```

``` r
generate_protein(5)
```

    [1] "FDFNS"

## Generate random protein sequences of length 6 to 13

> **Q4** Adapt and use your `generate_protein()` function to generate a
> series of random protein sequences ranging from 6 to 13 amino acids in
> length (one sequence per length). Take advantage of the base R
> function `for()` or `sapply()` so that you do not have to call
> generate_protein() eight times by hand.

``` r
for(l in 6:13) {
  cat(">", l, "\n", sep="")
  cat( generate_protein(l), "\n" )
}
```

    >6
    KGLWPH 
    >7
    TIINNRP 
    >8
    LRVNMENT 
    >9
    ENTGSHHNV 
    >10
    KHGLTKLGHD 
    >11
    FWENLPHEPCR 
    >12
    NNNIWPWAVWAC 
    >13
    NWVIYIAQDPWTR 

``` r
generate_protein(6)
```

    [1] "PVVQCP"

``` r
generate_protein(7)
```

    [1] "LLWYRVE"

``` r
generate_protein(8)
```

    [1] "GEDVALTF"

``` r
generate_protein(9)
```

    [1] "WVAFDKGHV"

## Are Our peptides “unique in nature”?

> **Q5**: Take your FASTA-formatted peptides from Q4 and run them as a
> single BLASTp search against the Non-redundant protein sequences (nr)
> database at https://blast.ncbi.nlm.nih.gov/. For this question do not
> restrict the organism (leave the Organism field blank so that the full
> nr database is searched).

| length | Ide | Cov | Unique |
|--------|-----|-----|--------|
| 6      | 100 | 100 | N      |
| 7      | 100 | 100 | N      |
| 8      | 100 | 88  | Y      |
| 9      | 100 | 89  | Y      |
| 10     | 90  | 100 | Y      |
| 11     | 90  | 91  | Y      |
| 12     | 77  | 100 | Y      |
| 13     | 100 | 69  | Y      |

## Connecting our findings to immunology

> **Q6**: In 3–6 sentences total and using your Q5 data and the
> reasoning from Q5b, what do you think this minimum length is and why
> might it be a bad design choice for the immune system to present very
> short peptides? \[3 pt\]
