# Validate DOI format

Validate DOI format

## Usage

``` r
doi_valid_format(doi)
```

## Arguments

- doi:

  a character vector of one or more DOIs

## Value

a logical vector

## Examples

``` r
doi_valid_format("10.1038/nphys1170")
#> [1] TRUE
doi_valid_format("no.no.10.1038")
#> [1] FALSE
```
