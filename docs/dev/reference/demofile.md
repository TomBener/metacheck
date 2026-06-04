# Get a demo file

Return the file path for various versions of the demo paper. Use
[`demopaper()`](https://scienceverse.github.io/metacheck/dev/reference/demopaper.md)
to directly read it as a paper object from the json file.

## Usage

``` r
demofile(ext = c("json", "pdf", "docx", "doc", "xml", "qmd"))
```

## Arguments

- ext:

  the extension of the file

## Value

file path

## Examples

``` r
json <- demofile()
pdf <- demofile("pdf")
```
