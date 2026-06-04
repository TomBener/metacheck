# Coerce bib authors column to list of data frames

Handles mixed input: structured `[{given, family}]` arrays (read as data
frames by jsonlite) and legacy pipe-separated strings. Returns a list
column suitable for storing in a data frame.

## Usage

``` r
.coerce_bib_authors(col)
```

## Arguments

- col:

  a list column (from jsonlite) or character vector of authors

## Value

a list where each element is a data frame with `given`/`family`
