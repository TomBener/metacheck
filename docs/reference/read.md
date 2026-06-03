# Read in grobid XML or bibr JSON

Read in grobid XML or bibr JSON

## Usage

``` r
read(file_path, include_images = FALSE, recursive = FALSE)
```

## Arguments

- file_path:

  path to a single directory containing XML and/or JSON files, or a
  vector of XML/JSON paths

- include_images:

  whether to include images in the figures table of the paper object
  (they make object size larger, only relevant to bibr imports)

- recursive:

  whether to read files in subfolders (files should have unique
  paper_ids, or errors can occur)

## Value

a paper or paperlist
