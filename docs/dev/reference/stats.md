# Check Stats

Check Stats

## Usage

``` r
stats(text, ...)
```

## Arguments

- text:

  the search table (or list of paper objects)

- ...:

  arguments to pass to statcheck()

## Value

a table of statistics

## Examples

``` r
paper <- demopaper()
stats(paper)
#>   test_type df1  df2 test_comp test_value p_comp reported_p computed_p
#> 1         t  NA 97.2         =      -1.96      =      0.152 0.05285936
#>                      raw error decision_error one_tailed_in_txt apa_factor
#> 1 t(97.2)=-1.96, p=0.152  TRUE          FALSE             FALSE          1
#>   text_id section_id paragraph_id
#> 1      22          8           11
#>                                                                                                                                                                                                              text
#> 1 On average researchers in the experimental condition found the app marginally significantly more useful (M=5.06) than researchers in the control condition found the checklist (M=4.5), t(97.2)=-1.96, p=0.152.
#>   formatted page_number        paper_id  header section_type
#> 1      <NA>           3 to_err_is_human Results      results
```
