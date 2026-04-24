# Query an LLM

Ask a large language model (LLM) any question you want about a vector of
text or the text from a search_text(). When `type` is provided, uses
ellmer's structured output API to guarantee output conforming to the
type spec; otherwise returns free-text responses in an `answer` column.

## Usage

``` r
llm(
  text,
  system_prompt,
  type = NULL,
  text_col = "text",
  model = llm_model(),
  params = list()
)
```

## Arguments

- text:

  The text to send to the LLM (vector of strings, or data frame with the
  text in a column)

- system_prompt:

  A system prompt to set the behavior of the assistant

- type:

  An optional ellmer type specification for structured extraction (e.g.,
  from `type_object()`, `type_from_schema()`). When provided, the
  provider enforces the schema and returns structured columns instead of
  free text.

- text_col:

  The name of the text column if text is a data frame

- model:

  the LLM model name (see
  [`llm_model_list()`](https://scienceverse.github.io/metacheck/dev/reference/llm_model_list.md))
  in the format "provider" or "provider/model"

- params:

  a named list to pass to
  [`ellmer::params()`](https://ellmer.tidyverse.org/reference/params.html)

## Value

a data frame of results

## Details

You will need to get your own API key from
<https://console.groq.com/keys>. To avoid having to type it out, add it
to the .Renviron file in the following format (you can use
[`usethis::edit_r_environ()`](https://usethis.r-lib.org/reference/edit.html)
to access the .Renviron file)

GROQ_API_KEY="key_value_asdf"

See <https://console.groq.com/docs> for more information

## Examples

``` r
if (FALSE) { # \dontrun{
# Free-text query
text <- c("hello", "number", "ten", 12)
system_prompt <- "Is this a number? Answer only 'TRUE' or 'FALSE'"
is_number <- llm(text, system_prompt)

# Structured extraction
type_spec <- ellmer::type_object(
  is_number = ellmer::type_boolean("Whether the input is a number")
)
result <- llm(c("hello", "42"), "Classify the input.", type = type_spec)
} # }
```
