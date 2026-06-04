# Get File List from GitHub

Get File List from GitHub

## Usage

``` r
github_files(repo, dir = "", recursive = FALSE)
```

## Arguments

- repo:

  The URL of the repository (in the format "username/repo" or
  "https://github.com/username/repo")

- dir:

  an optional directory name to search

- recursive:

  whether to search the files recursively

## Value

a data frame of files

## Examples

``` r
if (FALSE) { # \dontrun{
github_files("scienceverse/metacheck")
} # }
```
