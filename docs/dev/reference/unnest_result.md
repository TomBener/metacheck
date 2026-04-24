# Convert structured LLM result to a data frame

Handles single objects, wrapper objects with a single array field, and
data frames. Converts NULLs to NAs for data frame compatibility.

## Usage

``` r
unnest_result(result)
```

## Arguments

- result:

  a list from `chat$chat_structured()`

## Value

a data frame
