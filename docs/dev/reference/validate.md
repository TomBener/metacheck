# Validate

Validate

## Usage

``` r
validate(gt, module, compare = "table")
```

## Arguments

- gt:

  a data frame or vector of text

- module:

  the module

- compare:

  name of the module output table for comparison

## Value

something

## Examples

``` r
validate("p < .05", "stat_p_exact")
#>   paper_id    text text_id section_id paragraph_id header section_type p_comp
#> 1        1 p < .05       1          0            0   Test      unknown      <
#>   p_value expanded imprecise
#> 1    0.05  p < .05      TRUE
```
