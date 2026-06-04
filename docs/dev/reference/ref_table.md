# Reference and DOI table

Return a table with fixed DOIs and reference text from a paper object or
concatenate tables across a list of paper objects.

## Usage

``` r
ref_table(paper)
```

## Arguments

- paper:

  a paper or paperlist

## Value

a merged table

## Examples

``` r
biblio <- ref_table(psychsci[[1]])
```
