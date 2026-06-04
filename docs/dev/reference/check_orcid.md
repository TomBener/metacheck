# Check validity of ORCiD

Check validity of ORCiD

## Usage

``` r
check_orcid(orcid)
```

## Arguments

- orcid:

  a 16-character ORCiD in bare or URL format

## Value

a formatted 16-character ORCiD or FALSE

## Examples

``` r
check_orcid("0000-0002-7523-5539")
#> [1] "0000-0002-7523-5539"
check_orcid("0000-0002-0247-239X")
#> [1] "0000-0002-0247-239X"
check_orcid("https://orcid.org/0000-0002-0247-239X")
#> [1] "0000-0002-0247-239X"
check_orcid("0000-0002-0247-2394") # incorrect, return FALSE
#> Warning: The ORCiD 0000-0002-0247-2394 is not valid.
#> [1] FALSE
```
