# List modules

List modules

## Usage

``` r
module_list(module_dir = system.file("modules", package = "metacheck"))
```

## Arguments

- module_dir:

  the directory to search for modules (defaults to the built-in modules)

## Value

a data frame of modules

## Examples

``` r
mods <- module_list()
```
