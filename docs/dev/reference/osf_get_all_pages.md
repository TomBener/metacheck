# Get All OSF API Query Pages

OSF API queries only return up to 10 items per page, so this helper
functions checks for extra pages and returns all of them

## Usage

``` r
osf_get_all_pages(url, page_end = Inf)
```

## Arguments

- url:

  the OSF API URL

- page_end:

  The last page to get

## Value

a table of the returned data

## Examples

``` r
# get the 20 newest preprints
# \donttest{
osf_api <- getOption("metacheck.osf.api")
url <- sprintf("%s/preprints/?search=date_created-desc", osf_api)
preprints <- osf_get_all_pages(url, 2)
# }
```
