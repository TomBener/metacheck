# Get GitHub Repo Info

Get GitHub Repo Info

## Usage

``` r
github_info(repo, recursive = FALSE)
```

## Arguments

- repo:

  The URL of the repository (in the format "username/repo" or
  "https://github.com/username/repo")

- recursive:

  whether to search the files recursively

## Value

a list of information about the repo

## Examples

``` r
# \donttest{
github_info("scienceverse/metacheck")
#> $repo
#> [1] "scienceverse/metacheck"
#> 
#> $readme
#> [1] "# metacheck\n\n<!-- badges: start -->\n![Made in Europe](https://img.shields.io/badge/Made_in_Europe-003399?logo=european-union&logoColor=FFCC00)\n\n[![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)\n\n[![Codecov test coverage](https://codecov.io/gh/scienceverse/metacheck/graph/badge.svg)](https://app.codecov.io/gh/scienceverse/metacheck)\n<!-- badges: end -->\n\nThe goal of metacheck is to automatically check research outputs for best practices. You can find out more at <https://scienceverse.github.io/metacheck/>.\n\n## Installation\n\nYou can install the development version of metacheck from [GitHub](https://github.com/) with:\n\n``` r\n# install.packages(\"devtools\")\ndevtools::install_github(\"scienceverse/metacheck\")\n```\n\n## API (optional)\nTo run metacheck as a REST API either using plumber or Docker, see [`inst/plumber/README.md`](inst/plumber/README.md) for instructions and documentation.\n"
#> 
#> $files
#>                      repo             clean_repo               name
#> 1  scienceverse/metacheck scienceverse/metacheck      .Rbuildignore
#> 2  scienceverse/metacheck scienceverse/metacheck            .github
#> 3  scienceverse/metacheck scienceverse/metacheck         .gitignore
#> 4  scienceverse/metacheck scienceverse/metacheck         AUTHORS.md
#> 5  scienceverse/metacheck scienceverse/metacheck CODE_OF_CONDUCT.md
#> 6  scienceverse/metacheck scienceverse/metacheck    CONTRIBUTING.md
#> 7  scienceverse/metacheck scienceverse/metacheck    CONTRIBUTORS.md
#> 8  scienceverse/metacheck scienceverse/metacheck        DESCRIPTION
#> 9  scienceverse/metacheck scienceverse/metacheck         LICENSE.md
#> 10 scienceverse/metacheck scienceverse/metacheck          NAMESPACE
#> 11 scienceverse/metacheck scienceverse/metacheck            NEWS.md
#> 12 scienceverse/metacheck scienceverse/metacheck                  R
#> 13 scienceverse/metacheck scienceverse/metacheck          README.md
#> 14 scienceverse/metacheck scienceverse/metacheck   _metacheck.Rproj
#> 15 scienceverse/metacheck scienceverse/metacheck             _stuff
#> 16 scienceverse/metacheck scienceverse/metacheck        codecov.yml
#> 17 scienceverse/metacheck scienceverse/metacheck               data
#> 18 scienceverse/metacheck scienceverse/metacheck           data-raw
#> 19 scienceverse/metacheck scienceverse/metacheck               docs
#> 20 scienceverse/metacheck scienceverse/metacheck               inst
#> 21 scienceverse/metacheck scienceverse/metacheck           makefile
#> 22 scienceverse/metacheck scienceverse/metacheck                man
#> 23 scienceverse/metacheck scienceverse/metacheck            pkgdown
#> 24 scienceverse/metacheck scienceverse/metacheck              tests
#> 25 scienceverse/metacheck scienceverse/metacheck          vignettes
#>                  path
#> 1       .Rbuildignore
#> 2             .github
#> 3          .gitignore
#> 4          AUTHORS.md
#> 5  CODE_OF_CONDUCT.md
#> 6     CONTRIBUTING.md
#> 7     CONTRIBUTORS.md
#> 8         DESCRIPTION
#> 9          LICENSE.md
#> 10          NAMESPACE
#> 11            NEWS.md
#> 12                  R
#> 13          README.md
#> 14   _metacheck.Rproj
#> 15             _stuff
#> 16        codecov.yml
#> 17               data
#> 18           data-raw
#> 19               docs
#> 20               inst
#> 21           makefile
#> 22                man
#> 23            pkgdown
#> 24              tests
#> 25          vignettes
#>                                                                        download_url
#> 1       https://raw.githubusercontent.com/scienceverse/metacheck/main/.Rbuildignore
#> 2                                                                              <NA>
#> 3          https://raw.githubusercontent.com/scienceverse/metacheck/main/.gitignore
#> 4          https://raw.githubusercontent.com/scienceverse/metacheck/main/AUTHORS.md
#> 5  https://raw.githubusercontent.com/scienceverse/metacheck/main/CODE_OF_CONDUCT.md
#> 6     https://raw.githubusercontent.com/scienceverse/metacheck/main/CONTRIBUTING.md
#> 7     https://raw.githubusercontent.com/scienceverse/metacheck/main/CONTRIBUTORS.md
#> 8         https://raw.githubusercontent.com/scienceverse/metacheck/main/DESCRIPTION
#> 9          https://raw.githubusercontent.com/scienceverse/metacheck/main/LICENSE.md
#> 10          https://raw.githubusercontent.com/scienceverse/metacheck/main/NAMESPACE
#> 11            https://raw.githubusercontent.com/scienceverse/metacheck/main/NEWS.md
#> 12                                                                             <NA>
#> 13          https://raw.githubusercontent.com/scienceverse/metacheck/main/README.md
#> 14   https://raw.githubusercontent.com/scienceverse/metacheck/main/_metacheck.Rproj
#> 15                                                                             <NA>
#> 16        https://raw.githubusercontent.com/scienceverse/metacheck/main/codecov.yml
#> 17                                                                             <NA>
#> 18                                                                             <NA>
#> 19                                                                             <NA>
#> 20                                                                             <NA>
#> 21           https://raw.githubusercontent.com/scienceverse/metacheck/main/makefile
#> 22                                                                             <NA>
#> 23                                                                             <NA>
#> 24                                                                             <NA>
#> 25                                                                             <NA>
#>     size          ext   type
#> 1    331 rbuildignore   file
#> 2      0       github    dir
#> 3    380    gitignore config
#> 4    177           md   text
#> 5   5240           md   text
#> 6   4768           md   text
#> 7    133           md   text
#> 8   2243                file
#> 9  34303           md   text
#> 10  2895                file
#> 11 18431           md   text
#> 12     0                 dir
#> 13   995           md   text
#> 14   462        rproj config
#> 15     0                 dir
#> 16   134          yml config
#> 17     0                 dir
#> 18     0                 dir
#> 19     0                 dir
#> 20     0                 dir
#> 21  1407                file
#> 22     0                 dir
#> 23     0                 dir
#> 24     0                 dir
#> 25     0                 dir
#> 
#> $languages
#>                      repo   language    bytes
#> 1  scienceverse/metacheck       HTML 18689007
#> 2  scienceverse/metacheck          R  6447012
#> 3  scienceverse/metacheck        TeX    47751
#> 4  scienceverse/metacheck       AMPL     7571
#> 5  scienceverse/metacheck     Python     6986
#> 6  scienceverse/metacheck        CSS     3358
#> 7  scienceverse/metacheck   Makefile     1407
#> 8  scienceverse/metacheck Dockerfile     1201
#> 9  scienceverse/metacheck JavaScript     1018
#> 10 scienceverse/metacheck       SCSS      104
#> 11 scienceverse/metacheck      Shell       17
#> 
# }
```
