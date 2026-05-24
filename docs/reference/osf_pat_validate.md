# OSF PAT Validation

Checks for validity of the OSF PAT and unsets it if needed.

## Usage

``` r
osf_pat_validate(osf_pat = Sys.getenv("OSF_PAT"))
```

## Arguments

- osf_pat:

  the OSF PAT (read from renviron by default)

## Value

logical (TRUE if OSF_PAT is set and valid)
