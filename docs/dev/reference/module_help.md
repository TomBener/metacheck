# Get Module Help

See the help files for a module by name (get a list of names from
[`module_list()`](https://scienceverse.github.io/metacheck/dev/reference/module_list.md))

## Usage

``` r
module_help(module = NULL)
```

## Arguments

- module:

  the name of a module or path to a module

## Value

the help text

## Examples

``` r
module_help("marginal")
#> Marginal Significance
#> 
#> List all sentences that describe an effect as 'marginally significant'.
#> 
#> module_run(paper, "marginal")
#> 
#> - paper: a paper object or paperlist object  
#> 
#> The marginal module searches for regular expressions that match a predefined pattern. The list of terms is a subset of those listed in a [blog post by Matthew Hankins](https://web.archive.org/web/20251001114321/https://mchankins.wordpress.com/2013/04/21/still-not-significant-2/). The module returns all sentences that match terms describing ‘marginally significant’ results.
#> 
#> Some of the terms identified might not be problematic in some contexts, and there are ways to describe ‘marginal significance’ that are not detected by the module.
```
