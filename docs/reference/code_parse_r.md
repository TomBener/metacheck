# Parse code to check for errors

Parse code to check for errors

## Usage

``` r
code_parse_r(file_path = "", text = NULL)
```

## Arguments

- file_path:

  a vector of file paths to check

- text:

  alternative to file_path, pass text directly

## Value

a data frame with columns `file_path` and `line`

## Examples

``` r
file_path <- demofile("qmd")
code_parse_r(file_path)
#> # A tibble: 1 × 3
#>   file_path                                                          error msg  
#>   <chr>                                                              <lgl> <chr>
#> 1 C:/Users/dlakens/AppData/Local/R/win-library/4.5/metacheck/demos/… FALSE NA   
```
