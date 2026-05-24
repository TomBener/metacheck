# Retrieve info from AsPredicted by URL

Retrieve info from AsPredicted by URL

## Usage

``` r
aspredicted_retrieve(ap_url, id_col = 1, wait = 1)
```

## Arguments

- ap_url:

  an AsPredicted URL, or a table containing them (e.g., as created by
  [`aspredicted_links()`](https://scienceverse.github.io/metacheck/reference/aspredicted_links.md))

- id_col:

  the index or name of the column that contains AsPredicted URLs, if id
  is a table

- wait:

  wait time in seconds

## Value

a data frame of information
