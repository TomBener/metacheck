# Get paper schema

Read in the JSON schema for bibr formatted paper objects

## Usage

``` r
paper_schema()
```

## Value

The schema as a list

## Examples

``` r
schema <- paper_schema()
schema$`$defs`$info$required
#> [1] "title"        "keywords"     "doi"          "file_hash"    "input_format"
#> [6] "file_name"    "bibr_version"
```
