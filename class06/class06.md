# Class 6: R functions
Barry (PID: 911)

- [1. Function basics](#1-function-basics)
- [2. Generate DNA sequence](#2-generate-dna-sequence)
- [3. Generate Protein function](#3-generate-protein-function)

## 1. Function basics

Let’s start writting our first silly function to add some numbers:

Every R function has 3 things:

- name (we get to pick this)
- input arguments (there can loads of these sperated by a comma)
- the body (the R code that does the work)

``` r
add <- function(x, y=10, z=0){
  x + y + z
}
```

I can just use this function like any other function as long as R knows
about it (i.e. run the code chunk)

``` r
add(1, 100)
```

    [1] 101

``` r
add( x=c(1,2,3,4), y=100)
```

    [1] 101 102 103 104

``` r
add(1)
```

    [1] 11

Functions can have “required” input arguments and “optional” input
arguments. The optional arguments are defined with an equals default
value (`y=10`) in the function defination.

``` r
add(x=1, y=100, z=10)
```

    [1] 111

> Q. Write a function to return a DNA sequence of a user specified
> lenght? Call it `generate_dna()`

The `sample()` function can help here

``` r
#generate_dna <- function(size=5) { }

students <- c("jeff","jeremy", "peter")

sample(students, size = 5, replace=TRUE)
```

    [1] "peter" "jeff"  "jeff"  "peter" "peter"

## 2. Generate DNA sequence

Now work with `bases` rather than `students`

``` r
bases <- c("A", "C", "G", "T")
sample(bases, size=10, replace = TRUE)
```

     [1] "T" "G" "C" "A" "G" "G" "C" "C" "A" "T"

Now I have a working ‘snippet’ of code I can use this as the body of my
first function version here:

``` r
generate_dna <- function(size=5) { 
  bases <- c("A", "C", "G", "T")
  sample(bases, size=size, replace = TRUE)
}
```

``` r
generate_dna(100)
```

      [1] "A" "G" "A" "A" "C" "A" "G" "A" "T" "C" "T" "G" "T" "T" "A" "G" "A" "G"
     [19] "A" "T" "A" "G" "A" "G" "T" "T" "G" "G" "A" "T" "A" "A" "G" "G" "T" "C"
     [37] "A" "G" "A" "C" "T" "C" "G" "C" "T" "C" "T" "C" "T" "C" "T" "T" "A" "C"
     [55] "T" "T" "G" "G" "G" "T" "C" "C" "G" "T" "C" "A" "A" "G" "T" "T" "G" "T"
     [73] "C" "G" "C" "C" "T" "C" "G" "C" "A" "A" "A" "C" "A" "A" "G" "C" "G" "G"
     [91] "C" "G" "G" "G" "G" "G" "G" "G" "T" "A"

``` r
generate_dna()
```

    [1] "T" "C" "A" "T" "C"

I want the ability to return a sequence like “AGTACCTG” i.e. a one
element vector where the bases are all together.

``` r
generate_dna <- function(size=5, together=TRUE) { 
  bases <- c("A", "C", "G", "T")
  sequence <- sample(bases, size=size, replace = TRUE)

  if(together) {
    sequence <- paste(sequence, collapse = "")
  }
  return(sequence)
}
```

``` r
generate_dna()
```

    [1] "GTGTC"

``` r
generate_dna(together = F)
```

    [1] "A" "A" "T" "A" "C"

## 3. Generate Protein function

> Q. Write a protein sequence generating function that will return
> sequences of a user specifed length?

We can get the set of 20 natural amino-acids from the **bio3d** package.

``` r
aa <- bio3d::aa.table$aa1[1:20]
aa
```

     [1] "A" "R" "N" "D" "C" "Q" "E" "G" "H" "I" "L" "K" "M" "F" "P" "S" "T" "W" "Y"
    [20] "V"

and use this in our function

``` r
generate_protein <- function(size=6, together=TRUE) {

  ## Get the 20 amino-acids as a vector
  aa <- bio3d::aa.table$aa1[1:20]
  sequence <- sample(aa, size, replace = TRUE)

  ## Optionally return a singe element string
  if(together){
    sequence <- paste(sequence, collapse = "")
  }
  return(sequence)
}
```

``` r
generate_protein(together = F)
```

    [1] "C" "C" "C" "N" "Y" "T"

> Q. Generate random protein sequenes of length 6 to 12 amino acids.

``` r
generate_protein(7)
```

    [1] "KCCQPRW"

``` r
generate_protein(8)
```

    [1] "NSGHLVMM"

``` r
generate_protein(9)
```

    [1] "SEGAMFHSR"

``` r
# generate_protein(size=6:12)
```

We can fix this inability to generate multiple sequences by either
editing and adding to the function body code (e.g. a for loop) or by
using the R **apply** family of utility functions.

``` r
sapply(6:12, generate_protein)
```

    [1] "YCGILK"       "IENVDKY"      "GDLSDNFV"     "WSVLAGISA"    "YFIIHHIRFE"  
    [6] "KYWLATASWCM"  "NQLVQRPFFRTN"

It would cool and useful if I could get FASTA format output

``` r
ans <- sapply(6:12, generate_protein)
ans
```

    [1] "EQIAIV"       "QNYGDTT"      "VHIMKYGR"     "RHSNQGGKE"    "DKISFHLKQE"  
    [6] "AAWLLDRVVTP"  "QRSVFFKTNHVS"

``` r
cat(ans, sep="\n")
```

    EQIAIV
    QNYGDTT
    VHIMKYGR
    RHSNQGGKE
    DKISFHLKQE
    AAWLLDRVVTP
    QRSVFFKTNHVS

I want this to look like FASTA format with an ID line, e.g.

    >ID.6
    HLDWLV
    >ID.7
    VREAIQN
    >ID.8
    WPRSKACN

The functions `paste()` and `cat()` can help us here…

``` r
cat( paste(">ID.", 7:12, "\n", ans, sep=""), sep="\n" )
```

    >ID.7
    EQIAIV
    >ID.8
    QNYGDTT
    >ID.9
    VHIMKYGR
    >ID.10
    RHSNQGGKE
    >ID.11
    DKISFHLKQE
    >ID.12
    AAWLLDRVVTP
    >ID.7
    QRSVFFKTNHVS

``` r
id.line <- paste(">ID.",7:12, sep="")
id.line
```

    [1] ">ID.7"  ">ID.8"  ">ID.9"  ">ID.10" ">ID.11" ">ID.12"

``` r
id.line <- paste(">ID.",6:12, sep="")
seq.line <- paste(id.line, ans, sep="\n")
cat(seq.line, sep="\n", file="myseq.fa")
```

> Q. Determine if these sequences can be found in nature or are they
> unique? Why or why not?

I BLASTp searched my FASTA foramt sequencs against NR and found that
length 6, 7, 8, are not unique and can be found in the databases with
100% coverage and 100% identity.

Random sequences of length 9 and above are unique and can’t be found in
the databases.
