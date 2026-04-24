# Process a paper using the bibr API

Process a paper using the bibr API

## Usage

``` r
convert_bibr(
  file_path,
  save_dir = ".",
  api_url = "https://api.bibr.metacheck.app",
  api_key = Sys.getenv("BIBR_API"),
  include_figures = FALSE,
  start_page = 1,
  end_page = Inf
)
```

## Arguments

- file_path:

  Path to the document file, or a directory of documents

- save_dir:

  Path to a directory in which to save the JSON file

- api_url:

  Base URL of the API

- api_key:

  Key to access bibr

- include_figures:

  Whether to include base64-encoded figure images in the output (default
  FALSE)

- start_page:

  First page of the file to extract

- end_page:

  Last page of the file to extract

## Value

A list of parsed information
