# Find AsPredicted Links in Papers

Find AsPredicted Links in Papers

## Usage

``` r
aspredicted_links(paper)
```

## Arguments

- paper:

  a paper object or paperlist object

## Value

a table with the AsPredicted url in the first (href) column

## Examples

``` r
aspredicted_links(psychsci)
#> # A tibble: 78 × 4
#>    href                              link_text text_id paper_id        
#>    <chr>                             <chr>       <int> <chr>           
#>  1 https://aspredicted.org/ve2qn.pdf NA             25 0956797619876260
#>  2 https://aspredicted.org/ve2qn.pdf NA             35 0956797619876260
#>  3 https://aspredicted.org/ve2qn.pdf NA             93 0956797619876260
#>  4 https://aspredicted.org/ve2qn.pdf NA            103 0956797619876260
#>  5 https://aspredicted.org/ve2qn.pdf NA            124 0956797619876260
#>  6 https://aspredicted.org/mq97g.pdf NA            101 0956797620927967
#>  7 https://aspredicted.org/mq97g.pdf NA            147 0956797620927967
#>  8 https://aspredicted.org/4gf64.pdf NA             31 0956797620948821
#>  9 https://aspredicted.org/8a6ta.pdf NA             57 0956797620948821
#> 10 https://aspredicted.org/rz98j.pdf NA            143 0956797620948821
#> # ℹ 68 more rows
```
