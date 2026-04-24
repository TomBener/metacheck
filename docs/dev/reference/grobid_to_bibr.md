# Convert Grobid TEI XML file to bibr format

Convert Grobid TEI XML file to bibr format

## Usage

``` r
grobid_to_bibr(xml_file, save_path = ".", crossref_lookup = FALSE)
```

## Arguments

- xml_file:

  the XML file

- save_path:

  directory or file path to save to; set to NULL to return a paper
  object

- crossref_lookup:

  whether to look up references in crossref

## Value

a paper object
