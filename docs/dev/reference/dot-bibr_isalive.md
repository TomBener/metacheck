# Check bibr server status

Check bibr server status

## Usage

``` r
.bibr_isalive(api_url, api_key = Sys.getenv("SCIVRS_API_KEY"), error = TRUE)
```

## Arguments

- api_url:

  the URL to the bibr server

- api_key:

  the API key to use (NULL if local)

- error:

  whether to generate and error on failure

## Value

boolean
