# Get Code Composition Stats

Get Code Composition Stats

## Usage

``` r
code_line_stats(code_text, lang = c("R", "SPSS", "SAS", "Stata"))
```

## Arguments

- code_text:

  the code text for a single file

- lang:

  the language (we only currently handle R, SPSS, SAS, Stata)

## Value

list with items `total_lines`, `comment_lines`, `code_lines`, and
`percent_comment`

## Examples

``` r
code_text <- c(
  "library(dplyr)",
  "",
  "# this line is a comment",
  "a <- 1"
)
code_line_stats(code_text, "R")
#> $total_lines
#> [1] 3
#> 
#> $comment_lines
#> [1] 1
#> 
#> $code_lines
#> [1] 2
#> 
#> $percent_comments
#> [1] 0.3333333
#> 
```
