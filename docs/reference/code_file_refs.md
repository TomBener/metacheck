# Get files referenced in code

Get files referenced in code

## Usage

``` r
code_file_refs(code_text, lang = c("R", "SPSS", "SAS", "Stata"))
```

## Arguments

- code_text:

  the code text for a single file

- lang:

  the language (we only currently handle R, SPSS, SAS, Stata)

## Value

a vector of files that are referenced in the code

## Examples

``` r
code_text <- c(
  'source("functions.R")',
  'a <- "bread"',
  'b <- read.csv("file.csv")'
)
code_file_refs(code_text, "R")
#> [1] "functions.R" "file.csv"   
```
