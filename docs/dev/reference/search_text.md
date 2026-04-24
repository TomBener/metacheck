# Search text

Search the text of a paper or list of paper objects. Also works on the
table results of a `search_text()` call.

## Usage

``` r
search_text(
  paper,
  pattern = ".*",
  return = c("sentence", "paragraph", "section", "header", "match", "paper_id"),
  ignore.case = TRUE,
  fixed = FALSE,
  perl = FALSE,
  exclude = FALSE,
  search_header = FALSE,
  include_refs = FALSE
)
```

## Arguments

- paper:

  a paper object or a list of paper objects

- pattern:

  the regex pattern to search for, if a vector with length \> 1, the
  patterns will be searched separately and combined

- return:

  the kind of text to return, the full sentence, paragraph, header, or
  section that the text is in, or just the (regex) match, or all body
  text for a paper (paper_id)

- ignore.case:

  whether to ignore case when text searching

- fixed:

  logical. If TRUE, pattern is a string to be matched as is. Overrides
  all conflicting arguments.

- perl:

  logical. Should Perl-compatible regexps be used?

- exclude:

  should matches be included or excluded

- search_header:

  also search the header

- include_refs:

  whether to include the reference section in the search

## Value

a data frame of matches

## Details

The section argument can take a vector of section names, or a PERL
regular expression (use ".\*" to match all sections). Possible section
types are abstract, intro, method, results, discussion, references,
acknowledgment, funding, endnote, footnote, table, figure, and unknown.
The default includes all sections except references, tables and figures.

## Examples

``` r
paper <- demopaper()
all_text <- search_text(paper)
study <- search_text(paper, "study")
equations <- search_text(paper, "\\b\\S+\\s*(=|<)\\s*[0-9\\.]+", return = "match")
no_numbers <- search_text(paper, "\\d", exclude = TRUE)
```
