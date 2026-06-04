# Retrieve info from the OSF by ID

Retrieve info from the OSF by ID

## Usage

``` r
osf_info(osf_url, id_col = 1, recursive = FALSE, pb = NULL)
```

## Arguments

- osf_url:

  an OSF ID or URL, or a table containing them

- id_col:

  the index or name of the column that contains OSF IDs or URLs, if id
  is a table

- recursive:

  whether to retrieve all children

- pb:

  a progress bar passed from another function

## Value

a data frame of information

## Examples

``` r
if (FALSE) { # \dontrun{
# get info on one OSF node
osf_info("pngda")

# also get child nodes and files
osf_info("https://osf.io/6nt4v", recursive = TRUE)
} # }
```
