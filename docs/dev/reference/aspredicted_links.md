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

a table with the AsPredicted url in the first (text) column

## Examples

``` r
aspredicted_links(psychsci)
#> # A tibble: 55 × 8
#>    link_text text_id paper_id  text  section_id paragraph_id header section_type
#>    <chr>       <int> <chr>     <chr> <lgl>      <lgl>        <lgl>  <lgl>       
#>  1 NA             24 09567976… http… NA         NA           NA     NA          
#>  2 NA            105 09567976… http… NA         NA           NA     NA          
#>  3 NA            126 09567976… http… NA         NA           NA     NA          
#>  4 NA            107 09567976… http… NA         NA           NA     NA          
#>  5 NA            153 09567976… http… NA         NA           NA     NA          
#>  6 NA             31 09567976… http… NA         NA           NA     NA          
#>  7 NA            179 09567976… http… NA         NA           NA     NA          
#>  8 NA            179 09567976… http… NA         NA           NA     NA          
#>  9 NA            179 09567976… http… NA         NA           NA     NA          
#> 10 NA            179 09567976… http… NA         NA           NA     NA          
#> # ℹ 45 more rows
```
