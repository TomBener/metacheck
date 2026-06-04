# Check whether a DOI resolves

Checks the doi.org API to see if a DOI is registered and has an
associated URL (using `https://doi.org/api/handles`). Returns TRUE if it
does, FALSE if the DOI does not exist or does not have an associated
URL, and NA if the test failed. Clearly invalid DOIs (i.e. not starting
with "10.") will return FALSE without server requests.

## Usage

``` r
doi_resolves(doi, timeout = 10)
```

## Arguments

- doi:

  Character vector. One or more DOIs to check.

- timeout:

  Numeric. Request timeout in seconds. Default is `10`.

## Value

Logical vector. For each input DOI, returns TRUE if the DOI resolves,
FALSE if it does not resolve (or does not start with 10.), and NA if the
check failed.

## Examples

``` r
if (FALSE) { # \dontrun{
doi_resolves("10.1038/nphys1170") # Expected: TRUE
doi_resolves("10.1234/invalid.doi") # Expected: FALSE
} # }
```
