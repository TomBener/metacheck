# Find GitHub Links in Papers

GitHub links can be in PDFs in several ways.

## Usage

``` r
github_links(paper)
```

## Arguments

- paper:

  a paper object or paperlist object

## Value

a table with the GitHub url in the first (text) column

## Examples

``` r
github_links(psychsci)
#> # A tibble: 10 × 9
#>    text_id paragraph_id section_id text    page_number paper_id formatted header
#>      <int>        <int>      <int> <chr>         <int> <chr>    <chr>     <chr> 
#>  1      42           14          3 https:…          NA 0956797… NA        "Meth…
#>  2     160           38          9 https:…          NA 0956797… NA        "Proc…
#>  3     298           80         19 https:…          NA 0956797… NA        ""    
#>  4     285           79         15 github…          NA 0956797… NA        "Fund…
#>  5      45           14          3 https:…          NA 0956797… NA        "Open…
#>  6      82           30          9 github…          NA 0956797… NA        "DNAm…
#>  7     227           88         31 jmobri…          NA 0956797… NA        "Open…
#>  8      40           10          3 .com/r…          NA 0956797… NA        "Open…
#>  9     396          112         24 giacom…          NA 0956797… NA        "Open…
#> 10     396          112         24 tree/m…          NA 0956797… NA        "Open…
#> # ℹ 1 more variable: section_type <chr>
```
