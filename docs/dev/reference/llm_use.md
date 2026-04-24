# Set or get metacheck LLM use

Mainly for use in optional LLM workflows in modules, also checks if the
GROQ API key is set and returns false if it isn't.

## Usage

``` r
llm_use(llm_use = NULL, API_KEY = Sys.getenv("GROQ_API_KEY"))
```

## Arguments

- llm_use:

  if logical, sets whether to use LLMs

- API_KEY:

  your API key for the LLM

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
