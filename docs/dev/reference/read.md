# Read in grobid XML or bibr JSON

Read in grobid XML or bibr JSON

## Usage

``` r
read(file_path, include_images = FALSE)
```

## Arguments

- file_path:

  path to a directory containing XML and/or JSON files, or a vector of
  paths

- include_images:

  whether to include images in the figures table of the paper object
  (they make object size larger, only relevant to bibr imports)

## Value

a paper or paperlist
