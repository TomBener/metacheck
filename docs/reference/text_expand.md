# Expand text

If you have a table resulting from
[`text_search()`](https://scienceverse.github.io/metacheck/reference/text_search.md)
or a module return object, you can expand the text column to the full
sentence, paragraph, or section. You can also set `plus` and `minus` to
append and prepend sentences to the result (only when `expand_to` is
"sentence").

## Usage

``` r
text_expand(
  results_table,
  paper,
  expand_to = c("sentence", "paragraph", "div", "section"),
  plus = 0,
  minus = 0
)

expand_text(
  results_table,
  paper,
  expand_to = c("sentence", "paragraph", "div", "section"),
  plus = 0,
  minus = 0
)
```

## Arguments

- results_table:

  the table to expand

- paper:

  a metacheck paper object or a list of paper objects to look up the
  expanded text from

- expand_to:

  whether to expand to the sentence, paragraph, div, or section level

- plus:

  append additional sentences after the target expansion

- minus:

  prepend additional sentences before the target expansion

## Value

a results table with the expanded text

## Examples

``` r
# single paper search
paper <- demopaper()
res_tbl <- text_search(paper, "p =", return = "match")
expanded <- text_expand(res_tbl, paper)

# multiple paper search
papers <- psychsci
res_tbl <- text_search(papers, "replicate")
expanded <- text_expand(res_tbl, papers, plus = 1, minus = 1)
```
