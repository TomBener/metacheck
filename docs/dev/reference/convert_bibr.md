# Convert documents using bibr

Converts document files (PDF, DOC, DOCX) to structured JSON using the
bibr extraction service. Supports two backends: the Scienceverse
platform (`"scivrs"`) which uses a job queue with load balancing, and a
self-hosted bibr instance (`"selfhosted"`) for direct API access.

## Usage

``` r
convert_bibr(
  file_path,
  save_path = ".",
  backend = c("auto", "scivrs", "selfhosted"),
  api_key = NULL,
  api_url = NULL,
  include_figures = FALSE,
  start_page = 1,
  end_page = Inf,
  poll_interval = 2,
  timeout = 600
)
```

## Arguments

- file_path:

  Path to the document file, or a directory of documents

- save_path:

  Path to a directory in which to save the JSON file

- backend:

  Which backend to use: `"auto"` (default) detects from the available
  API key, `"scivrs"` uses the Scienceverse platform, `"selfhosted"`
  uses a direct bibr API instance.

- api_key:

  API key (scivrs backend only). A Bearer token starting with `sv_`,
  defaults to the `SCIVRS_API_KEY` env var. Ignored for the
  `"selfhosted"` backend, which requires no authentication.

- api_url:

  Base URL of the API. Defaults to the appropriate URL for the selected
  backend.

- include_figures:

  Whether to include base64-encoded figure images in the output (default
  FALSE)

- start_page:

  First page of the file to extract (default 1)

- end_page:

  Last page of the file to extract (default Inf for all pages)

- poll_interval:

  Seconds between status polls, scivrs backend only (default 2)

- timeout:

  Maximum seconds to wait for processing, scivrs backend only (default
  600)

## Value

Path(s) to the saved JSON file(s)

## Details

When `backend = "auto"` (the default), the `"scivrs"` backend is used if
`api_key` is provided or the `SCIVRS_API_KEY` environment variable is
set. Otherwise, `"selfhosted"` is used (no authentication required).

## Examples

``` r
if (FALSE) { # \dontrun{
# Auto-detect backend from environment variables
pdf <- demofile("pdf")
convert_bibr(pdf)

# Explicitly use Scienceverse platform
convert_bibr(pdf, backend = "scivrs")

# Use self-hosted bibr instance
convert_bibr(pdf, backend = "selfhosted")

# Extract specific pages
convert_bibr(pdf, start_page = 1, end_page = 10)

# Directory of papers
dir <- system.file("demo", package = "metacheck")
convert_bibr(dir, save_path = "results/")
} # }
```
