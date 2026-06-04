# Convert a PDF to Grobid XML

This function uses a public grobid server maintained by Patrice Lopez.
You can set up your own local grobid server following instructions from
<https://grobid.readthedocs.io/> and set the argument `api_url` to its
path (probably <http://localhost:8070>)

## Usage

``` r
convert_grobid(
  file_path,
  save_path = ".",
  api_url = "http://localhost:8070",
  start_page = -1,
  end_page = -1,
  consolidate_citations = 0,
  consolidate_header = 0,
  consolidate_funders = 0
)
```

## Arguments

- file_path:

  path to the PDF, a vector of paths, or a directory name that contains
  PDFs

- save_path:

  directory or file path to save to; set to NULL to return the XML
  directly

- api_url:

  the URL to the grobid server

- start_page:

  the first page of the PDF to read (defaults to -1 to read all pages)

- end_page:

  the last page of the PDF to read (defaults to -1 to read all pages)

- consolidate_citations:

  whether to fix/enhance citations

- consolidate_header:

  whether to fix/enhance paper info

- consolidate_funders:

  whether to fix/enhance funder info

## Value

XML object

## Details

Consolidation of citations, headers, and funders looks up these items in
CrossRef or another database to fix or enhance information (see
<https://grobid.readthedocs.io/en/latest/Consolidation/>). This can slow
down conversion. Consolidating headers is only useful for published
papers, and can be set to 0 for work in prep.
