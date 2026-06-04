# Detect Code Language

Detects code language used in files, only for languages metacheck
currently processes (R, SAS, SPSS, Stata).

## Usage

``` r
code_lang(file_name)
```

## Arguments

- file_name:

  a vector of file names

## Value

a vector of languages

## Examples

``` r
file_name <- "file.R"
code_lang(file_name)
#> [1] "R"

file_name <- c("file.Rmd", "file.SAS", "file.r", "file.qmd", "file.txt")
code_lang(file_name)
#> file.Rmd file.SAS   file.r file.qmd file.txt 
#>      "R"    "SAS"      "R"      "R"       NA 
```
