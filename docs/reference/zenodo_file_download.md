# Download all Zenodo Project Files

Creates a directory for the Zenodo ID and downloads all of the files
using a folder structure from the Zenodo project nodes and file storage
structure. Returns (invisibly) a data frame with file info.

## Usage

``` r
zenodo_file_download(
  zenodo_id,
  download_to = ".",
  max_file_size = 10,
  max_download_size = 100,
  pb = NULL
)
```

## Arguments

- zenodo_id:

  an Zenodo ID or URL

- download_to:

  path to download to

- max_file_size:

  maximum file size to download (in MB) - set to NULL or Inf for no
  restrictions

- max_download_size:

  maximum total size to download - set to NULL of Inf for no
  restrictions

- pb:

  a progress bar passed from another function

## Value

data frame of file info

## Details

You can limit downloads to only files under a specific size (defaults to
10MB) and only a maximum download size (largest files will be omitted
until total size is under the limit). Omitted files will be listed as
messages in verbose mode, and included in the returned data frame with
the downloaded column value set to FALSE.

## Examples

``` r
if (FALSE) { # \dontrun{
  zenodo_file_download("2591593")
} # }
```
