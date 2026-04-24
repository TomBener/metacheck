# Extract P-Values

List all equations in the text, returning the matched text (e.g., 'p =
0.04') and document location in a table.

## Usage

``` r
extract_equations(paper)
```

## Arguments

- paper:

  a paper object or paperlist object

## Value

a table

## Details

Note that this will not catch p-values reported like "the p-value is
0.03" because that results in a ton of false positives when papers
discuss p-value thresholds. If you need to detect text like that, use
the
[`search_text()`](https://scienceverse.github.io/metacheck/dev/reference/search_text.md)
function and a custom pattern.

This will catch most comparators like =\<\>~and most versions of
scientific notation like 5.0 x 10^-2 or 5.0e-2. If you find any formats
that are not correctly handled by this function, please contact the
author.

## Examples

``` r
paper <- demopaper()
p_values <- extract_equations(paper)
```
