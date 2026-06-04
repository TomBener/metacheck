# Make an html link

Make an html link

## Usage

``` r
link(url, text = url, new_window = TRUE, type = "")
```

## Arguments

- url:

  the URL to link to

- text:

  the text to link

- new_window:

  whether to open in a new window

- type:

  handle common links, like "doi" ()

## Value

string

## Examples

``` r
link("https://scienceverse.org")
#> [1] "<a href='https://scienceverse.org' target='_blank'>scienceverse.org</a>"
```
