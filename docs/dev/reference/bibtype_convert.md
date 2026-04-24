# Convert crossref/doi types to bibtex types

Convert crossref/doi types to bibtex types

## Usage

``` r
bibtype_convert(type)
```

## Arguments

- type:

  a vector of crossref types

## Value

a vector of bibtext types

## Examples

``` r
crossref_types <- c("book-part",
                    "journal-article",
                    "monograph",
                    NA,
                    "unmatched-type")
bibtype_convert(crossref_types)
#> [1] "inbook"         "article"        "book"           NA              
#> [5] "unmatched-type"
```
