# Process a paper using the Scienceverse platform API

Submits a document to the Scienceverse platform queue for extraction.
The platform runs bibr behind Arq workers with load balancing, and is
the recommended way to process papers. Use
[`convert_bibr`](https://scienceverse.github.io/metacheck/dev/reference/convert_bibr.md)
for direct bibr API access without the queue.

## Usage

``` r
platform_convert_bibr(
  file_path,
  save_dir = ".",
  api_url = "https://platform.metacheck.app",
  api_key = Sys.getenv("PLATFORM_API_KEY"),
  include_figures = FALSE,
  poll_interval = 2,
  timeout = 600
)
```

## Arguments

- file_path:

  Path to the document file, or a directory of documents

- save_dir:

  Path to a directory in which to save the JSON file

- api_url:

  Base URL of the Scienceverse platform API

- api_key:

  Platform API key (Bearer token, starts with `sv_`). Defaults to the
  `PLATFORM_API_KEY` environment variable.

- include_figures:

  Whether to include base64-encoded figure images in the output (default
  FALSE)

- poll_interval:

  Seconds between status polls (default 2)

- timeout:

  Maximum seconds to wait for processing (default 600)

## Value

Path(s) to the saved JSON file(s)

## Examples

``` r
if (FALSE) { # \dontrun{
# Single file
pdf <- system.file("demo/to_err_is_human.pdf", package = "metacheck")
platform_convert_bibr(pdf)

# Directory of papers
dir <- system.file("demo", package = "metacheck")
platform_convert_bibr(dir, save_dir = "results/")
} # }
```
