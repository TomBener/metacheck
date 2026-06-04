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
#>   test_type df1  df2 test_comp test_value p_comp reported_p  computed_p
#> 1         t  NA 97.7         =       2.90      =      0.005 0.004609391
#> 2         t  NA 97.2         =      -1.96      =      0.152 0.052859364
#>                         raw error decision_error one_tailed_in_txt apa_factor
#> 1  t(97.7) = 2.9, p = 0.005 FALSE          FALSE             FALSE          1
#> 2 t(97.2) = -1.96, p =0.152  TRUE          FALSE             FALSE          1
#>                                                                                                                                                                                                                     text
#> 1                         On average researchers in the experimental (app) condition made fewer mistakes (M = 9.12) than researchers in the control (checklist) condition (M = 10.9), t(97.7) = 2.9, p = 0.005, d =0.59.
#> 2 On average researchers in the experimental condition found the app marginally significantly more useful (M = 5.06) than researchers in the control condition found the checklist (M = 4.5), t(97.2) = -1.96, p =0.152.
#>   text_id paragraph_id section_id page_number formatted        paper_id
#> 1      15            4          3          NA      <NA> to_err_is_human
#> 2      16            5          3          NA      <NA> to_err_is_human
#>      header section_type
#> 1 Procedure       method
#> 2 Procedure       method
```
