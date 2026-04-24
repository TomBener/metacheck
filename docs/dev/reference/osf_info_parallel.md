# Batch retrieve info from the OSF by ID (parallel)

Retrieves info for multiple OSF IDs in parallel using
[`httr2::req_perform_parallel()`](https://httr2.r-lib.org/reference/req_perform_parallel.html).
5-char GUIDs are resolved via the `guids/` endpoint; 24-char waterbutler
IDs go to `files/` (with sequential fallback on 404).

## Usage

``` r
osf_info_parallel(osf_ids, pb = NULL)
```

## Arguments

- osf_ids:

  a vector of valid, pre-checked OSF IDs

- pb:

  a progress bar passed from another function

## Value

a data frame of information
