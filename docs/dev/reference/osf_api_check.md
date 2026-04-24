# Check OSF API Server Status

Check the status of the OSF API server.

## Usage

``` r
osf_api_check(osf_api = getOption("metacheck.osf.api"))
```

## Arguments

- osf_api:

  the OSF API to use (e.g., "https://api.osf.io/v2")

## Value

the OSF status

## Details

The OSF API server is down a lot, so it's often good to check it before
you run a bunch of OSF functions. When the server is down, it can take
several seconds to return an error, so scripts where you are checking
many URLs can take a long time before you realise they aren't working.

You can only make 100 API requests per hour, unless you authorise your
requests, when you can make 10K requests per day. The osf functions in
metacheck often make several requests per URL to get all of the info.
You can authorise them by creating an OSF token at
https://osf.io/settings/tokens and including the following line in your
.Renviron file:

OSF_PAT="replace-with-your-token-string"

## Examples

``` r
osf_api_check()
#> [1] "ok"
```
