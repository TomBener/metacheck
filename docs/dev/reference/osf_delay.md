# Set the OSF delay

Sometimes the OSF gets fussy if you make too many calls, so you can set
a delay of a few seconds before each call. Use `osf_delay()` to get or
set the OSF delay.

## Usage

``` r
osf_delay(delay = NULL)
```

## Arguments

- delay:

  the number of seconds to wait between OSF calls

## Examples

``` r
osf_delay()
#> [1] 0
```
