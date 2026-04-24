# Get module information

Get module information

## Usage

``` r
module_info(module)
```

## Arguments

- module:

  the name of a module or path to a module

## Value

a list of module info

## Examples

``` r
module_info("all_p_values")
#> $title
#> [1] "List All P-Values"
#> 
#> $description
#> [1] "List all p-values in the text, returning the matched text (e.g., 'p = 0.04') and document location in a table."
#> 
#> $details
#> [1] "Note that this will not catch p-values reported like \"the p-value is 0.03\" because that results in a ton of false positives when papers discuss p-value thresholds. If you need to detect text like that, use `search_text()` function and a custom pattern like \"\\\\bp(-| )?values?\\\\s+.{1,20}\\\\s+[0-9\\\\.]+\"\n\nThis will catch most comparators like =<>~≈≠≤≥≪≫ and most versions of scientific notation like 5.0 x 10^-2 or 5.0e-2. If you find any formats that are not correctly handled by this function, please contact the author."
#> 
#> $keywords
#> [1] "results"
#> 
#> $author
#> [1] "Lisa DeBruine (\\email{lisa.debruine@glasgow.ac.uk})"
#> 
#> $param
#> $param$name
#> [1] "paper"
#> 
#> $param$description
#> [1] "a paper object or paperlist object"
#> 
#> 
#> $returns
#> [1] "a list"
#> 
#> $arg_defaults
#> $arg_defaults$paper
#> 
#> 
#> 
#> $func_name
#> [1] "all_p_values"
#> 
```
