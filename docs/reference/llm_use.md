# Set or get metacheck LLM use

Mainly for use in optional LLM workflows in modules

## Usage

``` r
llm_use(llm_use = NULL)
```

## Arguments

- llm_use:

  if logical, sets whether to use LLMs

## Value

the current option value (logical)

## Examples

``` r
if (llm_use()) {
  print("We can use LLMs")
} else {
  print("We will not use LLMs")
}
#> [1] "We will not use LLMs"
```
