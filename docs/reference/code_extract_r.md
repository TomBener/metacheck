# Convert Rmd/qmd files to R code only

Convert Rmd/qmd files to R code only

## Usage

``` r
code_extract_r(
  file_path = NULL,
  save_path = NULL,
  documentation = 0,
  text = NULL
)
```

## Arguments

- file_path:

  a vector of file paths to check

- save_path:

  if NULL, returns a text vector, else a path to save to

- documentation:

  0:2 value to pass to knitr::purl

- text:

  alternative to file_path, pass text directly

## Value

a character vector

## Examples

``` r
file_path <- demofile("qmd")
code_text <- code_extract_r(file_path)
```
