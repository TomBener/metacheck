# Get Short GitHub Repo Name

Get Short GitHub Repo Name

## Usage

``` r
github_repo(repo)
```

## Arguments

- repo:

  The URL of the repository (in the format "username/repo" or
  "https://github.com/username/repo")

## Value

character string of short repo name

## Examples

``` r
github_repo("scienceverse/metacheck")
#> [1] "scienceverse/metacheck"
github_repo("https://github.com/scienceverse/metacheck/")
#> [1] "scienceverse/metacheck"
github_repo("https://github.com/scienceverse/metacheck.git")
#> [1] "scienceverse/metacheck"
```
