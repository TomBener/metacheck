# Clean DOIs

Clean DOIs

## Usage

``` r
doi_clean(doi)
```

## Arguments

- doi:

  a character vector of one or more DOIs

## Value

a character vector of cleaned DOIs (no https://doi.org or DOI:)

## Examples

``` r
doi_clean("https://doi.org/10.1038/nphys1170")
#> [1] "10.1038/nphys1170"
doi_clean("doi:10.1038/nphys1170")
#> [1] "10.1038/nphys1170"
doi_clean("DOI: 10.1038/nphys1170")
#> [1] "10.1038/nphys1170"
```
