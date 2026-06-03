# Check OSF IDs

Check if strings are valid OSF IDs, URLs, or waterbutler IDs. Basically
an improved wrapper for
[`osfr::as_id()`](https://docs.ropensci.org/osfr/reference/as_id.html)
that returns NA for invalid IDs in a vector.

## Usage

``` r
osf_check_id(osf_id)
```

## Arguments

- osf_id:

  a vector of OSF IDs or URLs

## Value

a vector of valid IDs, with NA in place of invalid IDs

## Examples

``` r
osf_check_id("pngda")
#> [1] "pngda"
osf_check_id("osf.io/pngda")
#> [1] "pngda"
osf_check_id("https://osf.io/pngda")
#> [1] "pngda"
osf_check_id("https://osf .io/png da") # rogue whitespace
#> [1] "pngda"
osf_check_id("pnda") # invalid
#> Warning: pnda is not a valid OSF ID
#> [1] NA
```
