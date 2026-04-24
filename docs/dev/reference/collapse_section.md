# Make Collapsible Section

A helper function for making module reports.

## Usage

``` r
collapse_section(
  text,
  title = "Learn More",
  callout = c("tip", "note", "warning", "important", "caution"),
  collapse = TRUE
)
```

## Arguments

- text:

  The text to put in the collapsible section; vectors will be collapse
  with line breaks between (e.g., into paragraphs)

- title:

  The title of the collapse header

- callout:

  the type of quarto callout block

- collapse:

  whether to collapse the block at the start

## Value

text

## Examples

``` r
text <- c("Paragraph 1...", "Paragraph 2...")
collapse_section(text) |> cat()
#> ::: {.callout-tip title="Learn More" collapse="true"}
#> 
#> Paragraph 1...
#> 
#> Paragraph 2...
#> 
#> :::
```
