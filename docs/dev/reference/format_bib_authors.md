# Format Bib Authors

Formats a structured author list (data frame with given/family columns)
as a display string.

## Usage

``` r
format_bib_authors(authors)
```

## Arguments

- authors:

  a data frame with `given` and `family` columns, or a list of such data
  frames

## Value

a character string (or vector) of formatted author names

## Examples

``` r
authors <- data.frame(given = c("Alice H.", "Wendy"),
                      family = c("Eagly", "Wood"))
format_bib_authors(authors)
#> [1] "Eagly, Alice H.; Wood, Wendy"
```
