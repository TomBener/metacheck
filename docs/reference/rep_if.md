# Replace If

Replace values if NULL, NA, or specified value

## Usage

``` r
rep_if(x, y, replace = NULL)
```

## Arguments

- x:

  For each `x[[i]]` if a value in `replace`, return `y[[i]]`; otherwise
  return `x[[i]]`.

- y:

  Replacement value(s); if not the same length as `x`, then values will
  be recycled

- replace:

  values in `x` to replace

## Examples

``` r
rep_if(list(NULL, 1), 2)
#> [[1]]
#> [1] 2
#> 
#> [[2]]
#> [1] 1
#> 
rep_if(list(NULL, 0, NULL), 1:3)
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> [1] 0
#> 
#> [[3]]
#> [1] 3
#> 
```
