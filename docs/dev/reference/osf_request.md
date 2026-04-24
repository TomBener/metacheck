# Build an OSF API request

Helper that constructs a standard OSF API request with headers, error
suppression, and retry on 429.

## Usage

``` r
osf_request(url)
```

## Arguments

- url:

  the full API URL

## Value

an httr2 request object
