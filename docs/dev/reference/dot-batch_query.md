# Batch query

Batch query

## Usage

``` r
.batch_query(
  urls,
  batch_size = 5,
  msg = "Batch Query",
  delay = 0.5,
  accept = "application/json",
  req_func = function(req) {
     req
 }
)
```

## Arguments

- urls:

  A vector of URLs

- batch_size:

  Size of each batch

- msg:

  Message to show in progress bar - set to NULL to omit progressbar

- delay:

  Courtesy delay between batches (in seconds)

- accept:

  The header type to accept

## Value

a list of responses
