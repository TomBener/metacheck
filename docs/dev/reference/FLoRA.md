# FORRT Replication Database (FLoRA)

FLoRA database containing DOIs of original studies and replications. Use
[`FLoRA_date()`](https://scienceverse.github.io/metacheck/dev/reference/FLoRA_date.md)
to find the date it was downloaded, and
[`FLoRA_update()`](https://scienceverse.github.io/metacheck/dev/reference/FLoRA_update.md)
to update it.

## Usage

``` r
FLoRA()
```

## Format

A data frame with 8 columns:

- doi_o:

  DOI of original study

- apa_ref_o:

  APA reference of original study

- doi_r:

  DOI of replication study (may be NA if url_r is provided)

- apa_ref_r:

  APA reference of replication study

- url_r:

  URL of replication study (used when DOI is not available)

- outcome:

  replication outcome

- outcome_quote:

  quote describing replication outcome

- type:

  replication or reproduction

## Source

<https://osf.io/9r62x/files/t4j8f>

## Value

a data frame

## Examples

``` r
FLoRA()
#> # A tibble: 1,502 × 8
#>    doi_o             apa_ref_o doi_r apa_ref_r url_r outcome outcome_quote type 
#>    <chr>             <chr>     <chr> <chr>     <chr> <chr>   <chr>         <chr>
#>  1 10.1002/(sici)10… Hsee, C.… 10.1… Klein, R… NA    failed  "The differe… repl…
#>  2 10.1002/(sici)10… Finucane… 10.1… Efendić,… NA    succes… "In two well… repl…
#>  3 10.1002/(sici)10… Hsee, C.… 10.1… Vonasch,… NA    succes… "We found su… repl…
#>  4 10.1002/acp.1376  Garry, M… 10.1… Ito, H.,… NA    succes… "Each of the… repl…
#>  5 10.1002/acp.2874  Riekki, … 10.1… Miyazaki… NA    succes… "These resul… repl…
#>  6 10.1002/ajmg.b.3… Beach, S… 10.1… Beach, S… NA    succes… "Replicating… repl…
#>  7 10.1002/bdm.492   Kogut, T… 10.1… Majumder… NA    failed  "The replica… repl…
#>  8 10.1002/bdm.586   Critcher… 10.1… Klein, R… NA    failed  "This result… repl…
#>  9 10.1002/bdm.586   Critcher… 10.1… Shanks, … NA    failed  "No statisti… repl…
#> 10 10.1002/ejsp.2013 Huang, Y… 10.1… Klein, R… NA    succes… "The coordin… repl…
#> # ℹ 1,492 more rows
```
