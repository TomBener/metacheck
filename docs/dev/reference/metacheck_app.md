# Launch Shiny App

Create a meta-study file interactively in a shiny app that runs locally
in RStudio or your web browser (recommended).

## Usage

``` r
metacheck_app(paper = NULL, quiet = FALSE, ...)
```

## Arguments

- paper:

  optional paper or paperlist to load

- quiet:

  whether to show the debugging messages in the console

- ...:

  arguments to pass to shiny::runApp

## Value

A paper object created or edited by the app

## Examples

``` r
if (FALSE) { # \dontrun{
s <- metacheck_app()
} # }
```
