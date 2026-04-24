# Find OSF Links in Papers

OSF links can be tricky to find in PDFs, since they can insert spaces in
odd places, and view-only links that contain a ? are often interpreted
as being split across sentences. This function is our best attempt at
catching and fixing them all.

## Usage

``` r
osf_links(paper)
```

## Arguments

- paper:

  a paper object or paperlist object

## Value

a table with the OSF url in the first (text) column

## Examples

``` r
osf_links(psychsci)
#> # A tibble: 674 × 9
#>    text_id paragraph_id section_id text    page_number paper_id formatted header
#>      <int>        <int>      <int> <chr>         <int> <chr>    <chr>     <chr> 
#>  1     206           58         15 "osf.i…          NA 0956797… NA        "Open…
#>  2     209           58         15 "osf.i…          NA 0956797… NA        "Open…
#>  3     253           73         20 "osf.i…          NA 0956797… NA        ""    
#>  4     226           78         27 "osf.i…          NA 0956797… NA        "Open…
#>  5     279           82         23 "osf .…          NA 0956797… NA        ""    
#>  6     282           82         23 "osf.i…          NA 0956797… NA        ""    
#>  7      73           28         11 "osf.i…          NA 0956797… NA        "Open…
#>  8      76           28         11 "osf .…          NA 0956797… NA        "Open…
#>  9     167           47         14 "osf.i…          NA 0956797… NA        "Open…
#> 10     243           74         19 "osf.i…          NA 0956797… NA        "Open…
#> # ℹ 664 more rows
#> # ℹ 1 more variable: section_type <chr>
```
