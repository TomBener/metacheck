# Call ollama native API with think support

ellmer routes ollama via the OpenAI-compatible /v1/ endpoint, which
ignores think=FALSE. This helper calls /api/chat directly where think is
honoured.

## Usage

``` r
.llm_ollama_native(
  text,
  system_prompt,
  model = NULL,
  think = FALSE,
  options = list(),
  base_url = Sys.getenv("OLLAMA_BASE_URL", "http://localhost:11434")
)
```

## Arguments

- text:

  The text to send to the LLM (vector of strings, or data frame with the
  text in a column)

- system_prompt:

  A system prompt to set the behavior of the assistant

- model:

  the ollama model

- think:

  whether to use thinking mode (very slow)

- options:

  further options to pass to to the model

- base_url:

  the local URL
