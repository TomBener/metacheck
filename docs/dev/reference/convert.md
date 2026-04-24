# Convert documents

Uses grobid or bibr to convert a file to paper format.

## Usage

``` r
convert(
  file_path,
  save_path = ".",
  method = c("auto", "bibr", "grobid", "xml"),
  ...
)
```

## Arguments

- file_path:

  Path to the document file, or a directory of documents

- save_path:

  Path to a directory in which to save the JSON file

- method:

  whether to use bibr, grobid, or grobid_to_bibr to convert a file (see
  Details)

- ...:

  further arguments to pass to convert_bibr, convert_grobid, or
  grobid_to_bibr

## Value

the path to the JSON file

## Details

Both bibr and grobid can handle PDF files. Only bibr can convert doc or
docx files. Already-converted grobid XML files can be converted to bibr
format (set crossref_lookup=TRUE to add a bib_match table). If the
file_path is a directory, the method will be xml if any XML files are
present, and bibr if only doc or docx files are present.
