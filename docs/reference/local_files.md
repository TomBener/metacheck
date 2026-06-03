# List Local Files

Lists all files in a local directory recursively and returns a data
frame compatible with the `repo_check` output table, for use with
`code_check`.

## Usage

``` r
local_files(path, recursive = FALSE)
```

## Arguments

- path:

  path to a local directory or file, or a vector of paths

- recursive:

  whether to search the files recursively

## Value

a data frame with columns `repo_url`, `file_name`, `file_url`,
`file_location`, `file_size`, `file_type`

## Examples

``` r
if (FALSE) { # \dontrun{
  local_files("my_project")
} # }
```
