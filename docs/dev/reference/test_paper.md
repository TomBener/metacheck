# Test paper

Create a paper object with the specified text (mainly for
testing/demos).

## Usage

``` r
test_paper(text = LETTERS, url = character(0))
```

## Arguments

- text:

  a vector of text to add

- url:

  a vector of URLs to add

## Value

a paper object

## Examples

``` r
# to test a paper with a specific URL
p <- test_paper("https://osf.io/abcde")
```
