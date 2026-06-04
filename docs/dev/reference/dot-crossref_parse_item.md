# Parse a CrossRef item into a data frame row

Parse a CrossRef item into a data frame row

## Usage

``` r
.crossref_parse_item(
  item,
  select = c("DOI", "type", "title", "author", "container-title", "volume", "issue",
    "page", "URL", "abstract", "year", "error")
)
```

## Arguments

- item:

  a list from CrossRef API response

- select:

  fields to select

## Value

a data frame
