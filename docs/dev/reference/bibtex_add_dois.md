# Add DOIs to a bib file

Uses OpenAlex to search for items that match the title and journal of
bibtex entries that don't have a DOI and adds them in.

## Usage

``` r
bibtex_add_dois(bibfile, save_to = NULL, strict = TRUE)
```

## Arguments

- bibfile:

  The file path to the .bib file

- save_to:

  The file to save the results to; if NULL, saves to bibfile name with
  \_doi appended

- strict:

  Should there be a single exact match for title and journal, if FALSE,
  gives the best match

## Value

a bib table in the bib2df format
