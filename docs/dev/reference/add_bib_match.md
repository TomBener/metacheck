# Match table from bib table

Match table from bib table

## Usage

``` r
add_bib_match(paper, min_score = 50)
```

## Arguments

- paper:

  a paper or paperlist object

- min_score:

  minimal score that is taken to be a reliable match

## Value

the paper or paperlist with bib_match table added

## Examples

``` r
if (FALSE) { # \dontrun{
paper <- demopaper()
paper$bib_match <- NULL # remove existing
paper2 <- add_bib_match(paper)
paper2$bib_match
} # }
```
