# Remove comments from code text

Remove comments from code text

## Usage

``` r
code_remove_comments(code_text, lang = c("R", "SPSS", "SAS", "Stata"))
```

## Arguments

- code_text:

  the code text for a single file

- lang:

  the language (we only currently handle R, SPSS, SAS, Stata)

## Value

the code_text minus comment lines

## Examples

``` r
code_text <- c(
  "# this is a comment",
  "",
  "x <- 'And this is code'"
)
code_text_nc <- code_remove_comments(code_text, "R")
```
