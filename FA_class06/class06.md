# Class06: R Functions
Fan Wu(PID:A15127541)

All functions in R have at least 3 things:

- A **name**, we picked this and use it to call the function.
- Input **arguments**, there can be multiple comma seperated input to
  the function.
- The **body**, lines of R code that do the work of the function.

Our first wee function:

``` r
add <- function(x, y = 1){
  x + y
}
```

Let’s test our function:

``` r
add(c(1,2,3),y=10)
```

    [1] 11 12 13

``` r
add(10,c(20,1130))
```

    [1]   30 1140

``` r
add(10)
```

    [1] 11

``` r
add(10, 10)
```

    [1] 20

## A second function

Let’s try something more interesting.Make a sequence generation tool.

The `sample()` function could be useful

``` r
sample(1:10, size = 3)
```

    [1] 10  4  5

change this to work with the nucleotides A C G and T, and return 3 of
them

``` r
n<- c("A","C","G","T")
sample(n, size = 15, replace = TRUE)
```

     [1] "A" "C" "G" "A" "C" "A" "T" "C" "T" "A" "A" "A" "T" "G" "C"

Turn this snippet into a function that return a user specific length DNA
sequence. Let’s call it `generate_dna()` …

``` r
generate_dna<-function(size=10, nucleotides = c("A","C","G","T"), fasta = FALSE){
  # generate random choice of nucleotides
  v<- sample(n, size, replace = TRUE)
  
  # print a message to user
  cat("Well done you!\n")
  
  # Make a single element vector of string, rather than a vector of many elements of nucleotides
  s<- paste(v, collapse = "")
  
  # logic that help decide the form of output
  if(fasta){
    return(s)
  }else{
    return (v)
  }
  
}
```

``` r
generate_dna(3)
```

    Well done you!

    [1] "C" "T" "T"

I want the option to return a single element character vector with my
sequence all together like this “GGATGAC”

``` r
generate_dna(10, fasta=TRUE)
```

    Well done you!

    [1] "ACTGCCCTTG"

## A more advanced example

Make a 3rd function that generates protein sequence of a use specified
length and format.

``` r
#' Generate a random protein sequence
#'
#' @param length Integer. Number of amino acids in the sequence.
#' @param output_type Character. "string" for a single sequence string, "vector" for a character vector.
#'
#' @return A protein sequence as a string or vector, depending on output_type.
#' @examples
#' generate_protein(10, "string")
#' generate_protein(8, "vector")
generate_protein <- function(length, output_type = "string") {
  amino_acids <- c("A", "R", "N", "D", "C", "Q", "E", "G", "H", "I", 
                   "L", "K", "M", "F", "P", "S", "T", "W", "Y", "V")
  if (!is.numeric(length) || length < 1 || length != as.integer(length)) {
    stop("length must be a positive integer")
  }
  if (!is.character(output_type) || !(output_type %in% c("string", "vector"))) {
    stop("output_type must be a character string: 'string' or 'vector'")
  }
  seq <- sample(amino_acids, length, replace = TRUE)
  if (output_type == "string") {
    return(paste(seq, collapse = ""))
  } else {
    return(seq)
  }
}
```

Try this out..

``` r
generate_protein(10)
```

    [1] "PGKSPGIWTY"

> Q. Please generate random protein sequences between lengths 5 and 12
> amino acids

- One approach is to do this by brute force calling our function for
  each length 5 to 12

- Another approach is to write a `for()` loop to iterate over the input
  valued 5 to 12

``` r
seq_lengths <- 6:12
for (i in seq_lengths){
  cat(">",i,"\n", sep="")
  cat(generate_protein(i))
  cat("\n")
}
```

    >6
    EHMTAE
    >7
    FWWIKLI
    >8
    TADDCLFH
    >9
    HTFHCRMQC
    >10
    KQYNCLKTEG
    >11
    CACMASLNLSV
    >12
    QWMARQTVPAWQ

- A very useful 3rd R specific approach is to use the `sapply()`
  function

``` r
sapply(5:12, generate_protein)
```

    [1] "VYILT"        "DIGNDE"       "AVPAHTE"      "CIHYNDQK"     "HLQRAVMGL"   
    [6] "SENENAWPFE"   "NAWATDLNMMP"  "AEAIYVAYGFLV"

> **Key-Point**: Writing functions in R is doable, but not easiet thing.
> Starting with a working snippet of code and then using LLM tools to
> improve and generalize your function code is a productive approach.
