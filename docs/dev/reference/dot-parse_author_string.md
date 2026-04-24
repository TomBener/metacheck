# Parse a legacy author string into a data frame

Handles formats like "Family, Given; Family2, Given2" or "Family, Given,
and Given2 Family2".

## Usage

``` r
.parse_author_string(s)
```

## Arguments

- s:

  a character string of authors

## Value

a data frame with `given` and `family` columns
