# Progress Bar

Progress Bar

## Usage

``` r
pb(total, format = "[:bar] :current/:total")
```

## Arguments

- total:

  total number of ticks

- format:

  The format of the progress bar

## Value

a function

## Details

Tokens

They can be used in the format argument when creating the progress bar.

:bar The progress bar itself. :current Current tick number. :total Total
ticks. :elapsed Elapsed time in seconds. :elapsedfull Elapsed time in
hh:mm:ss format. :eta Estimated completion time in seconds. :percent
Completion percentage. :rate Download rate, bytes per second. :tick_rate
Similar to :rate, but we don't assume that the units are bytes, we just
print the raw number of ticks per second. :bytes Shows :current,
formatted as bytes. Useful for downloads or file reads if you don't know
the size of the file in advance. :spin Shows a spinner that updates even
when progress is advanced by zero.

## Examples

``` r
bar <- pb(5)
for (i in 1:5) {
  Sys.sleep(.25)
  bar$tick()
}

bar <- pb(NA, "(:spin) Item :what")
for (i in 1:5) {
  Sys.sleep(.25)
  bar$tick(0, list(what = i))
}
bar$terminate()
```
