# Find ResearchBox Links in Papers

Find ResearchBox Links in Papers

## Usage

``` r
rbox_links(paper)
```

## Arguments

- paper:

  a paper object or paperlist object

## Value

a table with the ResearchBox url in the first (href) column

## Examples

``` r
rbox_links(psychsci)
#> # A tibble: 3 × 4
#>   href                                                link_text text_id paper_id
#>   <chr>                                               <chr>       <int> <chr>   
#> 1 https://researchbox.org/801                         NA             48 0956797…
#> 2 https://researchbox.org/801                         NA            220 0956797…
#> 3 https://researchbox.org/1150&PEER_REVIEW_passcode=… NA            301 0956797…
```
