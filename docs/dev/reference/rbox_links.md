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

a table with the ResearchBox url in the first (text) column

## Examples

``` r
rbox_links(psychsci)
#> # A tibble: 2 × 9
#>   text_id paragraph_id section_id text     page_number paper_id formatted header
#>     <int>        <int>      <int> <chr>          <int> <chr>    <chr>     <chr> 
#> 1      48           17          4 https:/…          NA 0956797… NA        State…
#> 2     309          108         31 https:/…          NA 0956797… NA        Open …
#> # ℹ 1 more variable: section_type <chr>
```
