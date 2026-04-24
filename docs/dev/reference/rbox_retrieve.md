# Retrieve info from ResearchBox by URL

Retrieve info from ResearchBox by URL

## Usage

``` r
rbox_retrieve(rb_url, id_col = 1, pb = NULL)
```

## Arguments

- rb_url:

  an ResearchBox URL, or a table containing them (e.g., as created by
  [`rbox_links()`](https://scienceverse.github.io/metacheck/dev/reference/rbox_links.md))

- id_col:

  the index or name of the column that contains ResearchBox URLs, if id
  is a table

- pb:

  a progress bar passed from another function

## Value

a data frame of information

## Examples

``` r
if (FALSE) { # \dontrun{
# get info on one OSF node
rbox_retrieve("https://researchbox.org/801")
} # }
```
