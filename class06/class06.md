# Class 6: R functions
Barry (PID: 911)

All functions in R have at least 3 things:

- A **name**, we pick this and use it to call the function.
- Input **aruguments**, there can be multiple comma seperated inputs to
  the function.
- The **body**, lines of R code that do the work of the function.

Our first wee function:

``` r
add <- function(x, y=1) {
  x + y
}
```

Let’s test our function

``` r
add(c(1,2,3), y=10 )
```

    [1] 11 12 13

``` r
add(10)
```

    [1] 11

``` r
add(10, 100)
```

    [1] 110

## A second function

Let’s try something more intresting. Make a sequence generation tool.

The `sample()` function could be useful here.

``` r
sample(1:10, size = 3)
```

    [1] 10  7  6

change this to work with the nucleotides A C G and T and return 3 of
them

``` r
n <- c("A","C","G","T")
sample(n, size=15, replace = TRUE)
```

     [1] "A" "C" "G" "T" "G" "A" "T" "A" "G" "A" "T" "T" "A" "T" "A"

Turn this snipet into a function that returns a user specifed length dna
sequence. Let’s call it `generate_dna()`…

``` r
generate_dna <- function(len=10, fasta=FALSE) {
  n <- c("A","C","G","T")
  v <- sample(n, size=len, replace = TRUE)

  # Make a single element vector
  s <- paste(v, collapse="")

  cat("Well done you!\n")

  if(fasta) {
    return( s )
  } else {
    return( v )
  }
}
```

``` r
generate_dna(5)
```

    Well done you!

    [1] "C" "C" "G" "C" "G"

``` r
s <- generate_dna(15)
```

    Well done you!

``` r
s
```

     [1] "G" "T" "A" "C" "G" "A" "C" "C" "T" "G" "C" "G" "C" "T" "G"

I want the option to return a singe elemet character vector with my
sequence all together like this: “GGAGTAC”

``` r
generate_dna(10, fasta=TRUE)
```

    Well done you!

    [1] "GTATTCACGT"

``` r
generate_dna(10, fasta=FALSE)
```

    Well done you!

     [1] "G" "T" "T" "G" "T" "C" "T" "G" "T" "T"

## A more advanced example

Make a thrid function that generates protein sequence of a user specifed
lenght and format.

``` r
generate_protein <- function(size=15, fasta=TRUE) {
  aa <- c("A", "R", "N", "D", "C", "Q", "E", "G", 
          "H", "I", "L", "K", "M", "F", "P", "S", 
          "T", "W", "Y", "V")
  
  seq <- sample(aa, size = size, replace = TRUE)

  if (fasta) {
    return(paste(seq, collapse = ""))
  } else {
    return(seq)
  }
}
```

Try this out…

``` r
generate_protein(10)
```

    [1] "ARFISKNGEL"

> Q. Generate random protein sequnces between lengths 5 and 12
> amino-acids.

``` r
generate_protein(5)
```

    [1] "RRHFV"

``` r
generate_protein(6)
```

    [1] "VRNVEC"

One apprach is to do this by brute force calling our function for each
length 5 to 12.

Another apprach is to write a `for()` loop to itterate over the input
valued 5 to 12

A very useful third R specific apprach is to use the `sapply()`
function.

``` r
seq_lengths <- 6:12
for (i in seq_lengths) {
  cat(">",i, "\n", sep="")
  cat( generate_protein(i) )
  cat("\n")
}
```

    >6
    WVCHSG
    >7
    DFHAQKH
    >8
    GAMYQRFC
    >9
    DMEFHYAVR
    >10
    SFSPQEGFMF
    >11
    VECTVWVWGGF
    >12
    WTFNMLFIEPSS

``` r
sapply(5:12, generate_protein)
```

    [1] "PLTKF"        "TVCQHA"       "MQMYLGS"      "IQHGAMHP"     "CHFDWGAYW"   
    [6] "YTQKIPFKLQ"   "TGEDKYKISWV"  "DCTAVGALRIML"

> **Key-Point**: Writing functions in R is doable but not the eaisest
> thing. Starting with a working snippet of code and then using LLM
> tools to improve and generalize your funtion code is a productive
> approach.
