# Get Code Library Lines

Returns the lines on which library/require calls exist. This is a helper
function for the code_check module.

## Usage

``` r
code_library_lines(code_text, lang = c("R", "SPSS", "SAS", "Stata"))
```

## Arguments

- code_text:

  the code text for a single file

- lang:

  the language (we only currently handle R, SPSS, SAS, Stata)

## Value

a data frame with columns `code` and `line` (the line numbers on which
library calls exist, after removing blank lines and comments)

## Examples

``` r
code_text <- c(
  "library(dplyr)",
  "",
  "# this line won't count",
  "library(tidyr)",
  "renv::install('metacheck')"
)
code_library_lines(code_text, "R")
#> # A tibble: 3 × 2
#>   code                        line
#>   <chr>                      <int>
#> 1 library(dplyr)                 1
#> 2 library(tidyr)                 2
#> 3 renv::install('metacheck')     3
```
