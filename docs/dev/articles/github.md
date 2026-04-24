# Exploring GitHub Repositories

``` r
library(metacheck)
#> 
#> 
#> *******************************************
#> ✅ Welcome to metacheck
#> For support and examples visit:
#> https://scienceverse.github.io/metacheck/
#> 
#> ⚠️ Set an email to use APIs like OpenAlex
#> metacheck::email('your@address.org')
#> 
#> ‼️ This is alpha software; please check any
#> results. False positives and negatives will
#> occur at unknown rates.
#> *******************************************
```

There are some built-in functions in metacheck for exploring GitHub
repositories. You can use these in custom modules.

## github_repo

The github functions all work with the following formats for referring
to repositories:

- `"{username}/{repo}"`  
- `"{username}/{repo}.git"`  
- `"https://github.com/{username}/{repo}.git"`  
- `"https://github.com/{username}/{repo}/{...}"`

The
[`github_repo()`](https://scienceverse.github.io/metacheck/dev/reference/github_repo.md)
function returns the simplified format of. repo name, and NULL if the
repository in inaccessible.

``` r
github_repo("https://github.com/scienceverse/metacheck.git")
#> [1] "scienceverse/metacheck"
```

``` r
github_repo("scienceverse/checkpaper")
#> NULL
```

## github_readme

Get the text of the readme file, regardless of the exact file name
(e.g., README vs README.md).

``` r
readme <- github_readme("scienceverse/metacheck")

cat(readme)
```

    #> # metacheck
    #> 
    #> <!-- badges: start -->
    #> ![Made in Europe](https://img.shields.io/badge/Made_in_Europe-003399?logo=european-union&logoColor=FFCC00)
    #> 
    #> [![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
    #> 
    #> [![Codecov test coverage](https://codecov.io/gh/scienceverse/metacheck/graph/badge.svg)](https://app.codecov.io/gh/scienceverse/metacheck)
    #> <!-- badges: end -->
    #> 
    #> The goal of metacheck is to automatically check research outputs for best practices. You can find out more at <https://scienceverse.github.io/metacheck/>.
    #> 
    #> ## Installation
    #> 
    #> You can install the development version of metacheck from [GitHub](https://github.com/) with:
    #> 
    #> ``` r
    #> # install.packages("devtools")
    #> devtools::install_github("scienceverse/metacheck")
    #> ```
    #> 
    #> ## API (optional)
    #> To run metacheck as a REST API either using plumber or Docker, see [`inst/plumber/README.md`](inst/plumber/README.md) for instructions and documentation.

## github_languages

You can retrieve the number of bytes dedicated to various coding
languages, as detected and classified by GitHub.

``` r
github_languages("scienceverse/metacheck")
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
```

## github_files

You can get a list of file names, their path, size, file extension, and
a guess at their type.

By default, you just retrieve the files and directories in the base
directory, non-recursively.

``` r
github_files("scienceverse/metacheck")
#>                      repo             clean_repo               name
#> 1  scienceverse/metacheck scienceverse/metacheck   _metacheck.Rproj
#> 2  scienceverse/metacheck scienceverse/metacheck             _stuff
#> 3  scienceverse/metacheck scienceverse/metacheck            .github
#> 4  scienceverse/metacheck scienceverse/metacheck         .gitignore
#> 5  scienceverse/metacheck scienceverse/metacheck      .Rbuildignore
#> 6  scienceverse/metacheck scienceverse/metacheck         AUTHORS.md
#> 7  scienceverse/metacheck scienceverse/metacheck CODE_OF_CONDUCT.md
#> 8  scienceverse/metacheck scienceverse/metacheck        codecov.yml
#> 9  scienceverse/metacheck scienceverse/metacheck    CONTRIBUTING.md
#> 10 scienceverse/metacheck scienceverse/metacheck    CONTRIBUTORS.md
#> 11 scienceverse/metacheck scienceverse/metacheck               data
#> 12 scienceverse/metacheck scienceverse/metacheck           data-raw
#> 13 scienceverse/metacheck scienceverse/metacheck        DESCRIPTION
#> 14 scienceverse/metacheck scienceverse/metacheck               docs
#> 15 scienceverse/metacheck scienceverse/metacheck               inst
#> 16 scienceverse/metacheck scienceverse/metacheck         LICENSE.md
#> 17 scienceverse/metacheck scienceverse/metacheck           makefile
#> 18 scienceverse/metacheck scienceverse/metacheck                man
#> 19 scienceverse/metacheck scienceverse/metacheck          NAMESPACE
#> 20 scienceverse/metacheck scienceverse/metacheck            NEWS.md
#> 21 scienceverse/metacheck scienceverse/metacheck            pkgdown
#> 22 scienceverse/metacheck scienceverse/metacheck                  R
#> 23 scienceverse/metacheck scienceverse/metacheck          README.md
#> 24 scienceverse/metacheck scienceverse/metacheck              tests
#> 25 scienceverse/metacheck scienceverse/metacheck          vignettes
#>                  path
#> 1    _metacheck.Rproj
#> 2              _stuff
#> 3             .github
#> 4          .gitignore
#> 5       .Rbuildignore
#> 6          AUTHORS.md
#> 7  CODE_OF_CONDUCT.md
#> 8         codecov.yml
#> 9     CONTRIBUTING.md
#> 10    CONTRIBUTORS.md
#> 11               data
#> 12           data-raw
#> 13        DESCRIPTION
#> 14               docs
#> 15               inst
#> 16         LICENSE.md
#> 17           makefile
#> 18                man
#> 19          NAMESPACE
#> 20            NEWS.md
#> 21            pkgdown
#> 22                  R
#> 23          README.md
#> 24              tests
#> 25          vignettes
#>                                                                        download_url
#> 1    https://raw.githubusercontent.com/scienceverse/metacheck/main/_metacheck.Rproj
#> 2                                                                              <NA>
#> 3                                                                              <NA>
#> 4          https://raw.githubusercontent.com/scienceverse/metacheck/main/.gitignore
#> 5       https://raw.githubusercontent.com/scienceverse/metacheck/main/.Rbuildignore
#> 6          https://raw.githubusercontent.com/scienceverse/metacheck/main/AUTHORS.md
#> 7  https://raw.githubusercontent.com/scienceverse/metacheck/main/CODE_OF_CONDUCT.md
#> 8         https://raw.githubusercontent.com/scienceverse/metacheck/main/codecov.yml
#> 9     https://raw.githubusercontent.com/scienceverse/metacheck/main/CONTRIBUTING.md
#> 10    https://raw.githubusercontent.com/scienceverse/metacheck/main/CONTRIBUTORS.md
#> 11                                                                             <NA>
#> 12                                                                             <NA>
#> 13        https://raw.githubusercontent.com/scienceverse/metacheck/main/DESCRIPTION
#> 14                                                                             <NA>
#> 15                                                                             <NA>
#> 16         https://raw.githubusercontent.com/scienceverse/metacheck/main/LICENSE.md
#> 17           https://raw.githubusercontent.com/scienceverse/metacheck/main/makefile
#> 18                                                                             <NA>
#> 19          https://raw.githubusercontent.com/scienceverse/metacheck/main/NAMESPACE
#> 20            https://raw.githubusercontent.com/scienceverse/metacheck/main/NEWS.md
#> 21                                                                             <NA>
#> 22                                                                             <NA>
#> 23          https://raw.githubusercontent.com/scienceverse/metacheck/main/README.md
#> 24                                                                             <NA>
#> 25                                                                             <NA>
#>     size          ext   type
#> 1    462        rproj config
#> 2      0                 dir
#> 3      0       github    dir
#> 4    380    gitignore config
#> 5    331 rbuildignore   file
#> 6    177           md   text
#> 7   5240           md   text
#> 8    134          yml config
#> 9   4768           md   text
#> 10   133           md   text
#> 11     0                 dir
#> 12     0                 dir
#> 13  2243                file
#> 14     0                 dir
#> 15     0                 dir
#> 16 34303           md   text
#> 17  1407                file
#> 18     0                 dir
#> 19  2895                file
#> 20 18431           md   text
#> 21     0                 dir
#> 22     0                 dir
#> 23   995           md   text
#> 24     0                 dir
#> 25     0                 dir
```

``` r
github_files("scienceverse/metacheck", dir = ".github")
#>                     repo             clean_repo           name
#> 1 scienceverse/metacheck scienceverse/metacheck     .gitignore
#> 2 scienceverse/metacheck scienceverse/metacheck     CODEOWNERS
#> 3 scienceverse/metacheck scienceverse/metacheck ISSUE_TEMPLATE
#> 4 scienceverse/metacheck scienceverse/metacheck      workflows
#>                     path
#> 1     .github/.gitignore
#> 2     .github/CODEOWNERS
#> 3 .github/ISSUE_TEMPLATE
#> 4      .github/workflows
#>                                                                       download_url
#> 1 https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/.gitignore
#> 2 https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/CODEOWNERS
#> 3                                                                             <NA>
#> 4                                                                             <NA>
#>   size       ext   type
#> 1    7 gitignore config
#> 2   41             file
#> 3    0              dir
#> 4    0              dir
```

You can also retrieve files recursively. Searching a large repository
recursively can take a while.

``` r
github_files("scienceverse/metacheck",
             dir = ".github",
             recursive = TRUE)
#>                      repo             clean_repo                  name
#> 1  scienceverse/metacheck scienceverse/metacheck            .gitignore
#> 2  scienceverse/metacheck scienceverse/metacheck            CODEOWNERS
#> 3  scienceverse/metacheck scienceverse/metacheck        ISSUE_TEMPLATE
#> 4  scienceverse/metacheck scienceverse/metacheck             workflows
#> 5  scienceverse/metacheck scienceverse/metacheck         bug_report.md
#> 6  scienceverse/metacheck scienceverse/metacheck            config.yml
#> 7  scienceverse/metacheck scienceverse/metacheck    feature_request.md
#> 8  scienceverse/metacheck scienceverse/metacheck wrong_check_result.md
#> 9  scienceverse/metacheck scienceverse/metacheck          pkgdown.yaml
#> 10 scienceverse/metacheck scienceverse/metacheck      teams-notify.yml
#> 11 scienceverse/metacheck scienceverse/metacheck    test-coverage.yaml
#> 12 scienceverse/metacheck scienceverse/metacheck   upload_packages.yml
#>                                            path
#> 1                            .github/.gitignore
#> 2                            .github/CODEOWNERS
#> 3                        .github/ISSUE_TEMPLATE
#> 4                             .github/workflows
#> 5          .github/ISSUE_TEMPLATE/bug_report.md
#> 6             .github/ISSUE_TEMPLATE/config.yml
#> 7     .github/ISSUE_TEMPLATE/feature_request.md
#> 8  .github/ISSUE_TEMPLATE/wrong_check_result.md
#> 9                .github/workflows/pkgdown.yaml
#> 10           .github/workflows/teams-notify.yml
#> 11         .github/workflows/test-coverage.yaml
#> 12        .github/workflows/upload_packages.yml
#>                                                                                                  download_url
#> 1                            https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/.gitignore
#> 2                            https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/CODEOWNERS
#> 3                                                                                                        <NA>
#> 4                                                                                                        <NA>
#> 5          https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/ISSUE_TEMPLATE/bug_report.md
#> 6             https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/ISSUE_TEMPLATE/config.yml
#> 7     https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/ISSUE_TEMPLATE/feature_request.md
#> 8  https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/ISSUE_TEMPLATE/wrong_check_result.md
#> 9                https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/workflows/pkgdown.yaml
#> 10           https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/workflows/teams-notify.yml
#> 11         https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/workflows/test-coverage.yaml
#> 12        https://raw.githubusercontent.com/scienceverse/metacheck/main/.github/workflows/upload_packages.yml
#>    size       ext   type
#> 1     7 gitignore config
#> 2    41             file
#> 3     0              dir
#> 4     0              dir
#> 5   469        md   text
#> 6    46       yml config
#> 7   417        md   text
#> 8   732        md   text
#> 9  1380      yaml config
#> 10  521       yml config
#> 11 1877      yaml config
#> 12 3362       yml config
```

## github_info

Get all of the information about a repository in one list object, with
items named “repo”, “readme”, “languages”, and “files”.

``` r
github_info("scienceverse/demo")
#> $repo
#> [1] "scienceverse/demo"
#> 
#> $readme
#> [1] "# demo\nFor use in testing functions\n"
#> 
#> $files
#>                repo        clean_repo           name           path
#> 1 scienceverse/demo scienceverse/demo         folder         folder
#> 2 scienceverse/demo scienceverse/demo good-example.R good-example.R
#> 3 scienceverse/demo scienceverse/demo      README.md      README.md
#>                                                              download_url size
#> 1                                                                    <NA>    0
#> 2 https://raw.githubusercontent.com/scienceverse/demo/main/good-example.R  227
#> 3      https://raw.githubusercontent.com/scienceverse/demo/main/README.md   36
#>   ext type
#> 1      dir
#> 2   r code
#> 3  md text
#> 
#> $languages
#>                repo language bytes
#> 1 scienceverse/demo        R   227
```
