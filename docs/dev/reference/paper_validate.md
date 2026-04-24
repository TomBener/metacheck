# Validate a Paper Object

Checks if a paper object conforms to the JSON schema.

## Usage

``` r
paper_validate(paper)
```

## Arguments

- paper:

  a paper object

## Value

TRUE or error

## Examples

``` r
paper <- list(paper_id = "Not a paper object")
tryCatch(
  paper_validate(paper),
  error = \(e) print(e$message)
)
#> [1] "The following tables are missing:\n info, author, text, section, url, bib, xref, figure, table, eq"

paper <- demopaper()
paper_validate(paper)
#> [1] TRUE
```
