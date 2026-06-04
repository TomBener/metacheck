# Retrieve info from Zenodo by URL

Retrieve info from Zenodo by URL

## Usage

``` r
zenodo_info(zenodo_url, id_col = 1, pb = NULL)
```

## Arguments

- zenodo_url:

  an Zenodo URL, or a table containing them (e.g., as created by
  [`zenodo_links()`](https://scienceverse.github.io/metacheck/dev/reference/zenodo_links.md))

- id_col:

  the index or name of the column that contains Zenodo URLs, if id is a
  table

- pb:

  a progress bar passed from another function

## Value

a data frame of information

## Examples

``` r
if (FALSE) { # \dontrun{
  # get info on one zenodo link
  zenodo_info("https://doi.org/10.5281/zenodo.18648142")
} # }
```
