# Find Zenodo Links in Papers

Find Zenodo Links in Papers

## Usage

``` r
zenodo_links(paper)
```

## Arguments

- paper:

  a paper object or paperlist object

## Value

a table with the Zenodo url in the first (text) column

## Examples

``` r
zenodo_links(psychsci)
#> # A tibble: 8 × 7
#>   href               link_text text_id paper_id zenodo_url zenodo_id zenodo_link
#>   <chr>              <chr>       <int> <chr>    <chr>      <chr>     <chr>      
#> 1 https://doi.org/1… NA             60 0956797… https://d… 2591593   https://do…
#> 2 https://doi.org/1… NA            187 0956797… https://d… 2591593   https://do…
#> 3 http://doi.org/10… NA            195 0956797… http://do… 3972408   https://do…
#> 4 https://doi.org/1… NA            126 0956797… https://d… 3543572   https://do…
#> 5 https://doi.org/1… NA            127 0956797… https://d… 3543296   https://do…
#> 6 https://doi.org/1… NA            305 0956797… https://d… 6855921   https://do…
#> 7 https://zenodo.or… NA            226 0956797… https://z… 7105933   https://do…
#> 8 https://zenodo.or… NA            227 0956797… https://z… 7186901   https://do…
```
