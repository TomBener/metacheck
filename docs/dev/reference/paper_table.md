# Paper tables

Return a table from a paper object or concatenate tables across a list
of paper objects.

## Usage

``` r
paper_table(paper, table, cols = NULL)
```

## Arguments

- paper:

  a paper or paperlist

- table:

  a table name

- cols:

  the columns to return from the table (default all columns)

## Value

a merged table

## Examples

``` r
biblio <- paper_table(psychsci[1:10], "bib")
xrefs <- paper_table(psychsci[1:10], "xref")
```
