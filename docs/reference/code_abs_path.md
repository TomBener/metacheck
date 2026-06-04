# Return Absolute Paths

Check code for the presence of absolute paths

## Usage

``` r
code_abs_path(code_text)
```

## Arguments

- code_text:

  the text of the code, excluding comments

## Value

a vector of absolute paths

## Examples

``` r
code_text <- c(
  "file <- 'C:/User/lakens/file.R'",
  "tmp <- '/User/lakens/file.html'",
  "convert(file, tmp)"
)
code_abs_path(code_text)
#> # A tibble: 2 × 2
#>   abs_path                line
#>   <chr>                  <int>
#> 1 C:/User/lakens/file.R      1
#> 2 /User/lakens/file.html     2
```
