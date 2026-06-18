# Extract Equations

List all equations in the text, returning the matched text (e.g., 't(28)
= 2.4', 'p = 0.04') and document location in a table. This is the
canonical extractor for reported statistics and effect sizes; modules
that need statistics should read from this table rather than re-scanning
the text.

## Usage

``` r
extract_eq(paper)
```

## Arguments

- paper:

  a paper object or paperlist object

## Value

a data frame with one row per equation and the columns `lhs` (the
statistic name, e.g. "t", "F", "p"), `df` (parenthetical degrees of
freedom such as "(28)" or "(2, 57)", otherwise NA), `comp` (the
comparator, e.g. "="), `rhs` (the reported value as text), `grp_id`
(groups equations in the same sentence), `text_id`, and `paper_id`.

## Details

This will catch most comparators like =\<\>~and most versions of
scientific notation like 5.0 x 10^-2 or 5.0e-2. If you find any formats
that are not correctly handled by this function, please contact the
author.

## Examples

``` r
paper <- demopaper()
equations <- extract_eq(paper)
```
