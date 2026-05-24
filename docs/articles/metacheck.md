# metacheck

## Installation

You can install the development version of metacheck from
[GitHub](https://github.com/scienceverse/metacheck) with:

``` r

# install.packages("devtools")
devtools::install_github("scienceverse/metacheck")
```

``` r

library(metacheck)
```

You can launch a shiny app, but this has limited features and is under
development.

``` r

metacheck::metacheck_app()
```

### Load from PDF

The function
[`convert()`](https://scienceverse.github.io/metacheck/reference/convert.md)
can read PDF files and save them in [JSON
format](https://www.scienceverse.org/schema/paper.json). This requires
an internet connection and takes a few seconds per paper, so should only
be done once and the results saved for later use.

``` r

pdf_file <- demofile("pdf")
json_file <- convert(file_path = pdf_file, save_path = "converted")
```

You can set up your own local grobid server following instructions from
<https://grobid.readthedocs.io/>. The easiest way is to use Docker.

``` bash
docker run --rm --init --ulimit core=0 -p 8070:8070 lfoppiano/grobid:0.9.0
```

Then you can set your api_url to the local path <http://localhost:8070>.

``` r

json_file <- convert(file_path = pdf_file, 
                     save_path = "converted",
                     method = "grobid",
                     api_url = "http://localhost:8070")
```

### Load from JSON

The function
[`read()`](https://scienceverse.github.io/metacheck/reference/read.md)
can read converted JSON files.

``` r

paper <- read(json_file)
```

### Load from non-PDF document

To take advantage of grobid’s ability to parse references and other
aspects of papers, for now the best way is to convert your papers to
PDF. We will introduce our custom backend, bibr, soon and this will be
able to convert DOC and DOCX files directly.

### Batch Processing

The functions
[`convert()`](https://scienceverse.github.io/metacheck/reference/convert.md)
and
[`read()`](https://scienceverse.github.io/metacheck/reference/read.md)
also work on a folder of files, returning a list of JSON file paths or
paper objects, respectively. The functions
[`text_search()`](https://scienceverse.github.io/metacheck/reference/text_search.md),
[`text_expand()`](https://scienceverse.github.io/metacheck/reference/text_expand.md)
and [`llm()`](https://scienceverse.github.io/metacheck/reference/llm.md)
also work on a list of paper objects.

## Paper Components

Paper objects contain a lot of structured information, including info,
references, and citations.

### Info

``` r

paper$info
```

    #>                                         title     keywords  doi
    #> 1 To Err is Human: An Empirical Investigation list(list()) <NA>
    #>          file_hash input_format           file_name bibr_version paper_type
    #> 1 a26373a4f28e3718          pdf to_err_is_human.pdf         10.0  empirical
    #>   paper_type_confidence         oecd_l1                           oecd_l2
    #> 1                     0 Social Sciences Psychology and Cognitive Sciences
    #>   oecd_confidence
    #> 1              NA

### Bibliography

The bibliography is provided in a tabular format.

``` r

paper$bib
```

| bib_id | text_id | bib_type | doi | title | authors | year | container | volume | issue | first_page | last_page |
|---:|---:|:---|:---|:---|:---|---:|:---|:---|:---|:---|:---|
| 1 | 29 | misc | 10.5281/zenodo.2669586 | Faux: Simulation for Factorial Designs | DeBruine, Lisa | 2025 |  | NA | NA | NA | NA |
| 2 | 30 | article | 10.1037/0003-066x.54.6.408 | The Origins of Sex Differences in Human Behavior: Evolved Dispositions Versus Social Roles | Eagly, Alice H., and Wendy Wood | 1999 | American Psychologist | 54 | 6 | 408 | 423 |
| 3 | 31 | article | 10.1177/0956797614520714 | Evil Genius? How Dishonesty Can Lead to Greater Creativity | Gino, Francesca, and Scott S. Wiltermuth | 2014 | Psychological Science | 25 | 4 | 973 | 981 |
| 4 | 32 | article |  | Equivalence Testing for Psychological Research | Lakens, Daniël | 2018 | Advances in Methods and Practices in Psychological Science | 1 | NA | 259 | 270 |
| 5 | 33 | article | 10.0000/0123456789 | Human Error Is a Symptom of a Poor Design | Smith, F. | 2021 | Journal of Journals | NA | NA | NA | NA |

### Cross-References

Cross-references are also provided in a tabular format, with `xref_id`
to match the bibliography table.

``` r

paper$xref
```

| xref_id | xref_type | contents                   | text_id |
|--------:|:----------|:---------------------------|--------:|
|       1 | table     | Table 1                    |      20 |
|       1 | figure    | Figure 1                   |      20 |
|       2 | figure    | Figure 2                   |      23 |
|       1 | foot      | 1                          |      10 |
|       2 | foot      | 2                          |      19 |
|       3 | bib       | (Gino and Wiltermuth 2014) |       6 |
|      NA | bib       | (Smithy, 2020)             |       7 |
|       1 | bib       | (DeBruine 2025)            |      20 |

### Batch

There are functions to combine the infomation from a list of papers,
like the `psychsci` built-in dataset of 250 open access papers from
Psychological Science.

``` r

paper_table(psychsci[1:5], "info", c("title", "doi"))
```

    #> # A tibble: 5 × 3
    #>   title                                                           doi   paper_id
    #>   <chr>                                                           <chr> <chr>   
    #> 1 Mirror neurons, originally discovered in macaque monkeys using… 10.1… 0956797…
    #> 2 Beyond Gist: Strategic and Incremental Information Accumulatio… 10.1… 0956797…
    #> 3 Serotonin and Social Norms: Tryptophan Depletion Impairs Socia… 10.1… 0956797…
    #> 4 Action-Specific Disruption of Perceptual Confidence             10.1… 0956797…
    #> 5 Emotional Vocalizations Are Recognized Across Cultures Regardl… 10.1… 0956797…

``` r

paper_table(psychsci[1:5], "bib") |>
  dplyr::filter(!is.na(doi), doi != "")
```

    #> # A tibble: 6 × 16
    #>   bib_type doi     title authors editors publisher  year volume issue first_page
    #>   <chr>    <chr>   <chr> <chr>   <chr>   <chr>     <int> <chr>  <chr> <chr>     
    #> 1 article  10.338… The … Zylber… ""      ""         2012 6      ""    NA        
    #> 2 article  10.103… Stro… Ekman,… ""      ""         1994 115    ""    268       
    #> 3 article  10.117… Cult… Gendro… ""      ""         2014 25     ""    911       
    #> 4 article  10.103… Is t… Russel… ""      ""         1994 115    ""    102       
    #> 5 article  10.108… Perc… Sauter… ""      ""         2010 63     ""    2251      
    #> 6 article  10.107… Cros… Sauter… ""      ""         2010 107    ""    2408      
    #> # ℹ 6 more variables: last_page <chr>, container <chr>, bib_id <int>,
    #> #   year_suffix <chr>, text_id <int>, paper_id <chr>

## Search Text

You can access a parsed table of the full text of the paper via
`paper$text`, but you may find it more convenient to use the function
[`text_search()`](https://scienceverse.github.io/metacheck/reference/text_search.md).
The defaults return a data table of each sentence, with the section
type, header, div, paragraph and sentence numbers, and file name. (The
section type is a best guess from the headers, so may not always be
accurate.)

``` r

text <- text_search(paper)
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type |
|---:|---:|---:|:---|:---|---:|:---|:---|:---|
| 1 | 1 | 1 | Daniel Lakens Lisa DeBruine Jakub Werner | NA | 1 | to_err_is_human | To Err is Human: An Empirical Investigation | unknown |
| 2 | 1 | 2 | 2026-02-22 | NA | 1 | to_err_is_human | To Err is Human: An Empirical Investigation | unknown |
| 3 | 2 | 3 | This paper demonstrates some good and poor practices for use with the {metacheck} R package. | NA | 1 | to_err_is_human | Abstract | abstract |
| 4 | 2 | 3 | All data are simulated. | NA | 1 | to_err_is_human | Abstract | abstract |
| 5 | 2 | 3 | The paper shows examples of (1) open and closed OSF links; (2a) citation of retracted papers, (2b) citations without a doi, (2c) citations with Pubpeer comments, (2d) citations in the FLoRA replication database, and (2e) missing/mismatched/incorrect citations and references; (3a) R files with code on GitHub that do not load libraries in one location, (3b) load files that are not shared in the repository, (3c) lack comments, and (3d) have absolute file paths; (4) imprecise reporting of non-significant p-values; (5) tests with and without effect sizes; (6) use of “marginally significant” to describe non-significant findings; (7) a power analysis reporting some of the essential attributes; and (8) retrieving information from preregistrations. | NA | 1 | to_err_is_human | Abstract | abstract |
| 6 | 3 | 4 | Although intentional dishonesty might be a successful way to boost creativity (Gino and Wiltermuth 2014), it is safe to say most mistakes researchers make are unintentional. | NA | 1 | to_err_is_human |  | intro |

### Pattern

You can search for a specific word or phrase by setting the `pattern`
argument. The pattern is a regex string by default; set `fixed = TRUE`
if you want to find exact text matches.

``` r

text <- text_search(paper, pattern = "metacheck")
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type |
|---:|---:|---:|:---|:---|---:|:---|:---|:---|
| 3 | 2 | 3 | This paper demonstrates some good and poor practices for use with the {metacheck} R package. | NA | 1 | to_err_is_human | Abstract | abstract |
| 9 | 3 | 4 | In this study we examine the usefulness of metacheck to improve best practices. | NA | 1 | to_err_is_human |  | intro |

### Return

Set `return` to one of “sentence”, “paragraph”, “section”, or “match” to
control what gets returned.

``` r

text <- text_search(paper, "GitHub", 
                    return = "paragraph")
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type |
|:---|---:|---:|:---|:---|:---|:---|:---|:---|
| NA | 2 | 3 | This paper demonstrates some good and poor practices for use with the {metacheck} R package. All data are simulated. The paper shows examples of (1) open and closed OSF links; (2a) citation of retracted papers, (2b) citations without a doi, (2c) citations with Pubpeer comments, (2d) citations in the FLoRA replication database, and (2e) missing/mismatched/incorrect citations and references; (3a) R files with code on GitHub that do not load libraries in one location, (3b) load files that are not shared in the repository, (3c) lack comments, and (3d) have absolute file paths; (4) imprecise reporting of non-significant p-values; (5) tests with and without effect sizes; (6) use of “marginally significant” to describe non-significant findings; (7) a power analysis reporting some of the essential attributes; and (8) retrieving information from preregistrations. | NA | NA | to_err_is_human | Abstract | abstract |
| NA | 6 | 7 | Data and analysis code is available on GitHub from <https://github.com/Lakens/to_err_is_human> and from <https://researchbox.org/4377>. Data is also available from <https://osf.io/5tbm9> and code is also available from <https://osf.io/629bx>. | NA | NA | to_err_is_human | Data Availability | endnote |

### Regex matches

You can also return just the matched text from a regex search by setting
`return = "match"`. The extra `...` arguments in
[`text_search()`](https://scienceverse.github.io/metacheck/reference/text_search.md)
are passed to [`grep()`](https://rdrr.io/r/base/grep.html), so
`perl = TRUE` allows you to use more complex regex, like below.

``` r

pattern <- "[a-zA-Z]\\S*\\s*(=|<)\\s*[0-9\\.,-]*\\d"
text <- text_search(paper, pattern, return = "match", perl = TRUE)
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type |
|---:|---:|---:|:---|:---|---:|:---|:---|:---|
| 19 | 7 | 8 | N=50 | NA | 2 | to_err_is_human | Power Analysis | method |
| 21 | 8 | 10 | M=9.12 | NA | 3 | to_err_is_human | Results | results |
| 21 | 8 | 10 | M=10.9 | NA | 3 | to_err_is_human | Results | results |
| 21 | 8 | 10 | t(97.7)=2.9 | NA | 3 | to_err_is_human | Results | results |
| 21 | 8 | 10 | p=0.005 | NA | 3 | to_err_is_human | Results | results |
| 21 | 8 | 10 | d=0.59 | NA | 3 | to_err_is_human | Results | results |
| 22 | 8 | 11 | M=5.06 | NA | 3 | to_err_is_human | Results | results |
| 22 | 8 | 11 | M=4.5 | NA | 3 | to_err_is_human | Results | results |
| 22 | 8 | 11 | t(97.2)=-1.96 | NA | 3 | to_err_is_human | Results | results |
| 22 | 8 | 11 | p=0.152 | NA | 3 | to_err_is_human | Results | results |
| 39 | 16 | 25 | pwr::pwr.t.test(n = 50 | NA | 2 | to_err_is_human | Footnote 2 | footnote |
| 39 | 16 | 25 | power = 0.8 | NA | 2 | to_err_is_human | Footnote 2 | footnote |

### Expand Text

You can expand the text returned by
[`text_search()`](https://scienceverse.github.io/metacheck/reference/text_search.md)
or a module with
[`text_expand()`](https://scienceverse.github.io/metacheck/reference/text_expand.md).

``` r

marginal <- text_search(paper, "marginal") |>
  text_expand(paper, plus = 1, minus = 1)

marginal[, c("text", "expanded")]
```

    #> # A tibble: 2 × 2
    #>   text                                                                  expanded
    #>   <chr>                                                                 <chr>   
    #> 1 "The paper shows examples of (1) open and closed OSF links; (2a) cit… "All da…
    #> 2 "On average researchers in the experimental condition found the app … "On ave…

## Large Language Models

You can query the extracted text of papers with LLMs using any models
supported by [ellmer](https://ellmer.tidyverse.org/).

### Setup

You will need to get **your own API key** (the one below is a fake
example) from your preferred provider
(e.g. <https://console.groq.com/keys>). To avoid having to type it out,
add it to the .Renviron file in the following format (you can use
[`usethis::edit_r_environ()`](https://usethis.r-lib.org/reference/edit.html)
to access the .Renviron file).

``` bash
GROQ_GPT_KEY="sk-proj-abcdefghijklmnopqrs0123456789ABCDEFGHIJKLMNOPQRS"
```

``` r

# useful if you aren't sure where this file is
usethis::edit_r_environ()
```

You can get or set the default LLM model with
[`llm_model()`](https://scienceverse.github.io/metacheck/reference/llm_model.md)
and access a list of the current available models using
[`llm_model_list()`](https://scienceverse.github.io/metacheck/reference/llm_model_list.md).

| platform | id | object | owned_by | context_window | max_completion_tokens | created_at |
|:---|:---|:---|:---|---:|---:|:---|
| groq | groq/compound | model | Groq | 131072 | 8192 | 2025-09-04 |
| groq | openai/gpt-oss-safeguard-20b | model | OpenAI | 131072 | 65536 | 2025-10-29 |
| groq | llama-3.3-70b-versatile | model | Meta | 131072 | 32768 | 2024-12-06 |
| groq | meta-llama/llama-prompt-guard-2-22m | model | Meta | 512 | 512 | 2025-05-30 |
| groq | openai/gpt-oss-20b | model | OpenAI | 131072 | 65536 | 2025-08-05 |

When you start metacheck for the first time, it will check for relevant
API keys in your Renviron and automatically set the model to use. You
can get or set this with
[`llm_model()`](https://scienceverse.github.io/metacheck/reference/llm_model.md).

``` r

llm_model() # get current model
llm_model("groq") # set to ellmer's default groq model
llm_model("groq/llama-3.3-70b-versatile") # set to specific openai model
```

### LLM Queries

You can query the extracted text of papers with LLMs. See
[`?llm`](https://scienceverse.github.io/metacheck/reference/llm.md) for
details of how to get and set up your API key, choose an LLM, and adjust
settings.

Use
[`text_search()`](https://scienceverse.github.io/metacheck/reference/text_search.md)
first to narrow down the text into what you want to query. Below, we
limited search to the first ten papers, and returned sentences that
contains the word “power” and at least one number. Then we asked an LLM
to determine if this is an a priori power analysis, and if so, to return
some relevant values in a JSON-structured format.

``` r

power <- psychsci[1:10] |>
  # sentences containing the word power
  text_search("power") |>
  # and containing at least one number
  text_search("[0-9]") 

# ask a specific question with specific response format
system_prompt <- 'Does this sentence report an a priori power analysis? If so, return the test, sample size, critical alpha criterion, power level, effect size and effect size metric plus any other relevant parameters, in JSON format like:

{
  "apriori": true, 
  "test": "paired samples t-test", 
  "sample": 20, 
  "alpha": 0.05, 
  "power": 0.8, 
  "es": 0.4, 
  "es_metric": "cohen\'s D"
}

If not, return {"apriori": false}

Answer only in valid JSON format, starting with { and ending with }.'

llm_power <- llm(power, system_prompt)
```

### Expand JSON

It is useful to ask an LLM to return data in JSON structured format, but
can be frustrating to extract the data, especially where the LLM makes
syntax mistakes. The function
[`json_expand()`](https://scienceverse.github.io/metacheck/reference/json_expand.md)
tries to expand a column with a JSON-formatted response into columns and
deals with it gracefully (sets an ‘error’ column to “parsing error”) if
there are errors. It also fixes column data types, if possible.

``` r

llm_response <- json_expand(llm_power, "answer") |>
  dplyr::select(text, apriori:es_metric)
```

| text | apriori | test | sample | alpha | power | es | es_metric |
|:---|:---|:---|---:|---:|---:|---:|:---|
| It is possible that less-consistent effects were observed on trials with errors because of reduced power to detect an effect on these trials, which by design were less numerous (~25%). | FALSE | NA | NA | NA | NA | NA | NA |
| Figure 1 shows that CY had very little predictive power for CLIM, but the fit in the transposed plot has an obvious bell-shaped curve. | FALSE | NA | NA | NA | NA | NA | NA |
| Sample size was calculated with an a priori power analysis, using the effect sizes reported by Küpper et al. (2014), who used identical procedures, materials, and dependent measures. | TRUE | NA | NA | NA | NA | NA | NA |
| We determined that a minimum sample size of 7 per group would be necessary for 95% power to detect an effect. | TRUE | t-test | 7 | 0.050 | 0.95 | NA | NA |
| For the first part of the task, 11 static visual images, one from each of the scenes in the film were presented once each on a black background for 2 s using Power-Point. | FALSE | NA | NA | NA | NA | NA | NA |
| A sample size of 26 per group was required to ensure 80% power to detect this difference at the 5% significance level. | TRUE | two-sample t-test | 26 | 0.050 | 0.80 | NA | NA |
| A sample size of 18 per condition was required in order to ensure an 80% power to detect this difference at the 5% significance level. | TRUE | t-test | 18 | 0.050 | 0.80 | NA | NA |
| The 13,500 selected loan requests conservatively achieved a power of .98 for an effect size of .07 at an alpha level of .05. | TRUE |  | 13500 | 0.050 | 0.98 | 0.07 | NA |
| On the basis of simulations over a range of expected effect sizes for contrasts of fMRI activity, we estimated that a sample size of 24 would provide .80 power at a conservative brainwide alpha threshold of .002 (although such thresholds ideally should be relaxed for detecting activity in regions where an effect is predicted). | TRUE | fMRI activity contrast | 24 | 0.002 | 0.80 | NA | NA |
| Stimulus sample size was determined via power analysis of the sole existing similar study, which used neural activity to predict Internet downloads of music (Berns & Moore, 2012). | TRUE | NA | NA | NA | NA | NA | NA |
| The effect size from that study implied that a sample size of 72 loan requests would be required to achieve .80 power at an alpha level of .05. | TRUE |  | 72 | 0.050 | 0.80 | NA | NA |
| Categorical ratings of the emotional expressions in the loan photographs had a similarly powerful impact on loan-request success; requests with “happy” photographs received \$5.15 more per hour than requests with “sad” photographs, on average; they achieved full funding in 7.6% less time. | FALSE | NA | NA | NA | NA | NA | NA |
| Although previous research has provided mixed evidence about the impact of positive versus negative affect on charitable giving (Andreoni, 1990;Small & Verrochi, 2009), by simultaneously assessing affect at both Internet-aggregate and laboratory-sample levels of analysis, our studies provide consistent evidence that photograph-elicited positive arousal most powerfully promoted lending rates and outcomes (Tables 1 and 2, Fig. 2a, and Fig. | FALSE | NA | NA | NA | NA | NA | NA |

### Rate Limiting

The [`llm()`](https://scienceverse.github.io/metacheck/reference/llm.md)
function makes a separate query [^1] for each row in a data frame from
[`text_search()`](https://scienceverse.github.io/metacheck/reference/text_search.md).
To prevent accidentally making way too many calls because of errors in
your code, we set the default limits to 30 queries at a time, but you
can change this:

``` r

llm_max_calls(30)
```

## OSF Functions

Metacheck provides several function to help you assess resources
archived on the Open Science Framework.

### OSF Links and IDs

Get any OSF links from a paper or list of papers.

``` r

links <- osf_links(psychsci)

links$href |> unique() |> head()
```

    #> [1] "https://osf.io/e2aks/"                                           
    #> [2] "https://osf.io/tvyxz/wiki/view/"                                 
    #> [3] "https://osf.io/t9j8e/?view_only=f171281f212f4435917b16a9e581a73b"
    #> [4] "https://osf.io/tvyxz/wiki/1.%20View%20the%20Badges/"             
    #> [5] "https://osf.io/eky4s/"                                           
    #> [6] "https://osf.io/xgwhk"

You can see that some of them have rogue spaces or view-only links. The
function
[`osf_check_id()`](https://scienceverse.github.io/metacheck/reference/osf_check_id.md)
takes most formats of OSF links (with or without <https://> and osf.io/,
as well as the 25-character waterbutler IDs) and converts them to short
IDs.

``` r

osf_ids <- osf_check_id(links$href) |> unique()

head(osf_ids)
```

    #> [1] "e2aks" "tvyxz" "t9j8e" "eky4s" "xgwhk" "5t0b7"

However, all of the `osf_***()` functions fix IDs for you and handle
duplicate IDs without making extra API calls, so you don’t need to add
this step to most workflows.

### OSF Info

Get basic information about OSF links, such as the name, description,
osf_type (nodes, files, preprints, registrations, users, set to
“private” if you don’t have authorisation to view it, and “invalid” if
the ), whether it is public

``` r

info <- osf_info(links[1:6, "href"])

info[, c("href","osf_id", "osf_type", "public", "category")]
```

    #> # A tibble: 6 × 5
    #>   href                                           osf_id osf_type public category
    #>   <chr>                                          <chr>  <chr>    <lgl>  <chr>   
    #> 1 https://osf.io/e2aks/                          e2aks  nodes    TRUE   project 
    #> 2 https://osf.io/tvyxz/wiki/view/                tvyxz  nodes    TRUE   project 
    #> 3 https://osf.io/tvyxz/wiki/view/                tvyxz  nodes    TRUE   project 
    #> 4 https://osf.io/t9j8e/?view_only=f171281f212f4… t9j8e  private  FALSE  NA      
    #> 5 https://osf.io/tvyxz/wiki/1.%20View%20the%20B… tvyxz  nodes    TRUE   project 
    #> 6 https://osf.io/eky4s/                          eky4s  nodes    TRUE   project

For now, the OSF API does not let us retrieve any information about
view-only links. They may be viewable by you in the web browser if the
link is still active, but will be listed in the table as public = FALSE
and osf_type = “private”.

You can set the argument `recursive = TRUE` to also retrieve information
about all nodes and files that are contained by the OSF link.

``` r

osf_api_calls(0)
all_contents <- osf_info(links$href[1], recursive = TRUE)
n_calls <- osf_api_calls()
```

The function
[`osf_api_calls()`](https://scienceverse.github.io/metacheck/reference/osf_api_calls.md)
lets you reset and retrieve the number of API calls made since the last
reset. You can see that the project <https://osf.io/e2aks/> had 3 nodes
and 6 files, which required 10 API calls.

``` r

sum(all_contents$osf_type == "nodes")
```

    #> [1] 3

### Download OSF Files

OSF projects let you organise information into nested components, and
files within those components. Therefore, to retrieve all of the files
associate with a project, you may need to navigate to several components
and download zip files for the files from each components, then
reorganise and rename the downloaded folders.

The function
[`osf_file_download()`](https://scienceverse.github.io/metacheck/reference/osf_file_download.md)
does all of this for you, recreating a folder structure based on the
component names and downloading all files smaller than `max_file_size`
(defaults to 10 MB) up to a total size of `max_download_size` (defaults
to 100 MB).

``` r

osf_file_download(osf_id = "pngda",
                  download_to = ".", 
                  max_file_size = 1, 
                  max_download_size = 10)
```

    Starting retrieval for pngda
    - omitting metacheck.png (1.5MB)
    Downloading files [=====================] 24/24 00:00:35

``` r

list.files("pngda", recursive = TRUE)
```

    #>  [1] "Data/Individual/data-01.csv"                         
    #>  [2] "Data/Individual/data-02.csv"                         
    #>  [3] "Data/Individual/data-03.csv"                         
    #>  [4] "Data/Individual/data-04.csv"                         
    #>  [5] "Data/Individual/data-05.csv"                         
    #>  [6] "Data/Individual/data-06.csv"                         
    #>  [7] "Data/Individual/data-07.csv"                         
    #>  [8] "Data/Individual/data-08.csv"                         
    #>  [9] "Data/Individual/data-09.csv"                         
    #> [10] "Data/Individual/data-10.csv"                         
    #> [11] "Data/Individual/data-11.csv"                         
    #> [12] "Data/Individual/data-12.csv"                         
    #> [13] "Data/Individual/data-13.csv"                         
    #> [14] "Data/Individual/data-14.csv"                         
    #> [15] "Data/Processed Data/processed-data.csv"              
    #> [16] "Data/Raw Data/data.xlsx"                             
    #> [17] "Data/Raw Data/nest-1/nest-2/nest-3/nest-4/test-4.txt"
    #> [18] "Data/Raw Data/nest-1/nest-2/nest-3/test-3.txt"       
    #> [19] "Data/Raw Data/nest-1/nest-2/test-2.txt"              
    #> [20] "Data/Raw Data/nest-1/README"                         
    #> [21] "Data/Raw Data/nest-1/test-1.txt"                     
    #> [22] "Data/Raw Data/README"                                
    #> [23] "README"

## Modules

metacheck is designed modularly, so you can add modules to check for
anything. It comes with a set of pre-defined modules, and we hope people
will share more modules.

### Module List

You can see the list of built-in modules with the function below.

``` r

module_list()
```

    #> 
    #> *** GENERAL ***
    #> 
    #> * all_urls: List all the URLs in the main text.
    #> * coi_check: Identify and extract Conflicts of Interest (COI) statements.
    #> * coi_check_oi: Identify and extract Conflicts of Interest (COI) statements.
    #> * funding_check: Identify and extract funding statements.
    #> * funding_check_oi: Identify and extract funding statements.
    #> * open_practices: This module incorporates ODDPub into metacheck. ODDPub is a text mining algorithm that detects which publications disseminated Open Data or Open Code together with the publication.
    #> 
    #> *** METHOD ***
    #> 
    #> * causal_claims: Aims to identify the presence of random assignment, and lists sentences that make causal claims in title or abstract.
    #> * power: This module uses uses regular expressions to identify sentences that contain a statistical power analysis. If specified by the user, it also uses a large language module (LLM) to extract information reported in power analyses, including the statistical test, sample size, alpha level, desired level of power, and magnitude and type of effect size.
    #> * prereg_check: Retrieve information from preregistrations in a standardised way,
    #> and make them easier to check.
    #> 
    #> *** RESULTS ***
    #> 
    #> * all_p_values: List all p-values in the text, returning the matched text (e.g., 'p = 0.04') and document location in a table.
    #> * code_check: This module retrieves information from repositories checked by repo_check about code files (R, SAS, SPSS, Stata).
    #> * marginal: List all sentences that describe an effect as 'marginally significant'.
    #> * repo_check: This module retrieves information from repositories.
    #> * stat_check: Check consistency of p-values and test statistics
    #> * stat_effect_size: The Effect Size module checks for effect sizes in t-tests and F-tests.
    #> * stat_p_exact: List any p-values reported with insufficient precision (e.g., p < .05 or p = n.s.)
    #> * stat_p_nonsig: This module checks for imprecisely reported p values. If p > .05 is detected, it warns for misinterpretations.
    #> 
    #> *** REFERENCE ***
    #> 
    #> * ref_accuracy: This module checks references for mismatches with CrossRef.
    #> * ref_consistency: Check if all references are cited and all citations are referenced
    #> * ref_miscitation: Check for frequently miscited papers. This module is just a proof of concept -- the miscite database is not yet populated with real examples.
    #> * ref_pubpeer: This module checks references and warns for citations that have comments on pubpeer (excluding Statcheck comments).
    #> * ref_replication: This module checks references and warns for citations of original studies for which replication or reproduction studies exist in the FLoRA database.
    #> * ref_retraction: This module checks references and warns for citations in the RetractionWatch Database.
    #> * ref_summary: Summarise information about each reference in a paper.
    #> 
    #> Use `module_help("module_name")` for help with a specific module

### Running modules

To run a built-in module on a paper, you can reference it by name.

``` r

p <- module_run(paper, "all_p_values")
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type | p_comp | p_value |
|---:|---:|---:|:---|:---|---:|:---|:---|:---|:---|---:|
| 21 | 8 | 10 | p=0.005 | NA | 3 | to_err_is_human | Results | results | = | 0.005 |
| 22 | 8 | 11 | p=0.152 | NA | 3 | to_err_is_human | Results | results | = | 0.152 |
| 23 | 8 | 12 | p \> .05 | NA | 3 | to_err_is_human | Results | results | \> | 0.050 |

### Creating modules

You can create your own modules using R code. Modules can also contain
instructions for reporting, to give “traffic lights” for whether a check
passed or failed, and to include appropriate text feedback in a report.
See the [modules
vignette](https://scienceverse.github.io/metacheck/articles/modules.md)
for more details.

## Reports

You can generate a report from any set of modules. Check the function
help for the default set.

``` r

report(paper, output_format = "qmd")
```

See the [example
report](https://scienceverse.github.io/metacheck/report-example.md).

[^1]: Using the parallel functions in ellmer can be more efficient, but
    currently doesn’t do a good job of associating structured output to
    the input text when input may have 0+ outputs.
