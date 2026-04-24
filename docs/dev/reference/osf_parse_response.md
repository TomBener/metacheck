# Parse an OSF API response into a data frame

Parse an OSF API response into a data frame

## Usage

``` r
osf_parse_response(resp, id, req_type = "guids", pb = NULL)
```

## Arguments

- resp:

  an httr2 response

- id:

  the OSF ID that was requested

- req_type:

  the endpoint type used ("guids" or "files")

- pb:

  a progress bar

## Value

a single-row data frame
