# Read in a grobid XML file

Read in, strip TEI namespace, and fix common problems.

## Usage

``` r
.xml_read_grobid(path)
```

## Arguments

- path:

  Path to a grobid/TEI XML file

## Value

a list with class "xml_document" for xml2

## Examples

``` r
path <- demofile("xml")
xml <- .xml_read_grobid(path)
```
