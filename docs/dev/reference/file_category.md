# Categorise files

Categorise files

## Usage

``` r
file_category(contents)
```

## Arguments

- contents:

  a table with columns name, path such as from `osf_contents()`

## Value

the table with new column file_category

## Examples

``` r
contents <- c("script.R", "data.csv", "README", "codebook.csv")
file_category(contents)
#>           name filetype file_category
#> 1     script.R     code          code
#> 2     data.csv     data          data
#> 3       README                 readme
#> 4 codebook.csv     data      codebook
```
