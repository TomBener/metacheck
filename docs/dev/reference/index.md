# Package index

## Reading in Papers

- [`metacheck_app()`](https://scienceverse.github.io/metacheck/dev/reference/metacheck_app.md)
  : Launch Shiny App
- [`convert()`](https://scienceverse.github.io/metacheck/dev/reference/convert.md)
  : Convert documents
- [`convert_bibr()`](https://scienceverse.github.io/metacheck/dev/reference/convert_bibr.md)
  : Convert documents using bibr
- [`convert_grobid()`](https://scienceverse.github.io/metacheck/dev/reference/convert_grobid.md)
  : Convert a PDF to Grobid XML
- [`grobid_to_bibr()`](https://scienceverse.github.io/metacheck/dev/reference/grobid_to_bibr.md)
  : Convert Grobid TEI XML file to bibr format
- [`read()`](https://scienceverse.github.io/metacheck/dev/reference/read.md)
  : Read in grobid XML or bibr JSON
- [`demopaper()`](https://scienceverse.github.io/metacheck/dev/reference/demopaper.md)
  : Get demo paper
- [`demofile()`](https://scienceverse.github.io/metacheck/dev/reference/demofile.md)
  : Get a demo file
- [`psychsci`](https://scienceverse.github.io/metacheck/dev/reference/psychsci.md)
  : Psychological Science Open Access Paper Set
- [`test_paper()`](https://scienceverse.github.io/metacheck/dev/reference/test_paper.md)
  : Test paper
- [`paper_id()`](https://scienceverse.github.io/metacheck/dev/reference/paper_id.md)
  : Get Paper IDs
- [`paper_write()`](https://scienceverse.github.io/metacheck/dev/reference/paper_write.md)
  : Write paper
- [`paper_validate()`](https://scienceverse.github.io/metacheck/dev/reference/paper_validate.md)
  : Validate a Paper Object

## Search Papers

- [`text_search()`](https://scienceverse.github.io/metacheck/dev/reference/text_search.md)
  [`search_text()`](https://scienceverse.github.io/metacheck/dev/reference/text_search.md)
  : Search text
- [`text_expand()`](https://scienceverse.github.io/metacheck/dev/reference/text_expand.md)
  [`expand_text()`](https://scienceverse.github.io/metacheck/dev/reference/text_expand.md)
  : Expand text
- [`paper_table()`](https://scienceverse.github.io/metacheck/dev/reference/paper_table.md)
  : Paper tables
- [`ref_table()`](https://scienceverse.github.io/metacheck/dev/reference/ref_table.md)
  : Reference and DOI table

## Module Functions

- [`module_list()`](https://scienceverse.github.io/metacheck/dev/reference/module_list.md)
  : List modules
- [`module_help()`](https://scienceverse.github.io/metacheck/dev/reference/module_help.md)
  : Get Module Help
- [`module_info()`](https://scienceverse.github.io/metacheck/dev/reference/module_info.md)
  : Get module information
- [`module_run()`](https://scienceverse.github.io/metacheck/dev/reference/module_run.md)
  : Run a module
- [`module_report()`](https://scienceverse.github.io/metacheck/dev/reference/module_report.md)
  : Report from module output
- [`module_template()`](https://scienceverse.github.io/metacheck/dev/reference/module_template.md)
  : Create a Module from a Template
- [`report()`](https://scienceverse.github.io/metacheck/dev/reference/report.md)
  : Create a Report
- [`report_module_run()`](https://scienceverse.github.io/metacheck/dev/reference/report_module_run.md)
  : Run modules for a report
- [`report_qmd()`](https://scienceverse.github.io/metacheck/dev/reference/report_qmd.md)
  : Create Report from Module Output

## Module Helpers

These functions help module writers make consistently formatted report
text.

- [`scroll_table()`](https://scienceverse.github.io/metacheck/dev/reference/scroll_table.md)
  : Make Scroll Table
- [`report_table()`](https://scienceverse.github.io/metacheck/dev/reference/report_table.md)
  : Display a Table in a Report
- [`collapse_section()`](https://scienceverse.github.io/metacheck/dev/reference/collapse_section.md)
  : Make Collapsible Section
- [`plural()`](https://scienceverse.github.io/metacheck/dev/reference/plural.md)
  : Pluralise
- [`link()`](https://scienceverse.github.io/metacheck/dev/reference/link.md)
  : Make an html link
- [`logger()`](https://scienceverse.github.io/metacheck/dev/reference/logger.md)
  : Log messages
- [`lastlog()`](https://scienceverse.github.io/metacheck/dev/reference/lastlog.md)
  : Get the last log
- [`format_ref()`](https://scienceverse.github.io/metacheck/dev/reference/format_ref.md)
  : Format Reference
- [`get_prev_outputs()`](https://scienceverse.github.io/metacheck/dev/reference/get_prev_outputs.md)
  : Get Previous Outputs
- [`format_bib_authors()`](https://scienceverse.github.io/metacheck/dev/reference/format_bib_authors.md)
  : Format Bib Authors
- [`fig_image_view()`](https://scienceverse.github.io/metacheck/dev/reference/fig_image_view.md)
  : View a figure image

## Code Check Helpers

These functions help repo_check and code_check modules to assess code
files.

- [`code_abs_path()`](https://scienceverse.github.io/metacheck/dev/reference/code_abs_path.md)
  : Return Absolute Paths
- [`code_extract_r()`](https://scienceverse.github.io/metacheck/dev/reference/code_extract_r.md)
  : Convert Rmd/qmd files to R code only
- [`code_file_refs()`](https://scienceverse.github.io/metacheck/dev/reference/code_file_refs.md)
  : Get files referenced in code
- [`code_lang()`](https://scienceverse.github.io/metacheck/dev/reference/code_lang.md)
  : Detect Code Language
- [`code_library_lines()`](https://scienceverse.github.io/metacheck/dev/reference/code_library_lines.md)
  : Get Code Library Lines
- [`code_line_stats()`](https://scienceverse.github.io/metacheck/dev/reference/code_line_stats.md)
  : Get Code Composition Stats
- [`code_parse_r()`](https://scienceverse.github.io/metacheck/dev/reference/code_parse_r.md)
  : Parse code to check for errors
- [`code_read()`](https://scienceverse.github.io/metacheck/dev/reference/code_read.md)
  : Read code from files
- [`code_remove_comments()`](https://scienceverse.github.io/metacheck/dev/reference/code_remove_comments.md)
  : Remove comments from code text

## LLM Functions

- [`llm()`](https://scienceverse.github.io/metacheck/dev/reference/llm.md)
  : Query an LLM
- [`llm_max_calls()`](https://scienceverse.github.io/metacheck/dev/reference/llm_max_calls.md)
  : Set the maximum number of calls to the LLM
- [`llm_model()`](https://scienceverse.github.io/metacheck/dev/reference/llm_model.md)
  : Set the default LLM model
- [`llm_model_list()`](https://scienceverse.github.io/metacheck/dev/reference/llm_model_list.md)
  : List LLM Models
- [`llm_use()`](https://scienceverse.github.io/metacheck/dev/reference/llm_use.md)
  : Set or get metacheck LLM use
- [`json_expand()`](https://scienceverse.github.io/metacheck/dev/reference/json_expand.md)
  : Expand a JSON column

## References/Citations

- [`add_bib_match()`](https://scienceverse.github.io/metacheck/dev/reference/add_bib_match.md)
  : Match table from bib table
- [`crossref_doi()`](https://scienceverse.github.io/metacheck/dev/reference/crossref_doi.md)
  : CrossRef Info from DOI
- [`crossref_query()`](https://scienceverse.github.io/metacheck/dev/reference/crossref_query.md)
  : Look up Reference in CrossRef
- [`datacite_doi()`](https://scienceverse.github.io/metacheck/dev/reference/datacite_doi.md)
  : Doi.org Info from DataCite
- [`doi_clean()`](https://scienceverse.github.io/metacheck/dev/reference/doi_clean.md)
  : Clean DOIs
- [`doi_lookup()`](https://scienceverse.github.io/metacheck/dev/reference/doi_lookup.md)
  : Doi.org Info from DOI
- [`doi_valid_format()`](https://scienceverse.github.io/metacheck/dev/reference/doi_valid_format.md)
  : Validate DOI format
- [`doi_resolves()`](https://scienceverse.github.io/metacheck/dev/reference/doi_resolves.md)
  : Check whether a DOI resolves
- [`FLoRA()`](https://scienceverse.github.io/metacheck/dev/reference/FLoRA.md)
  : FORRT Replication Database (FLoRA)
- [`FLoRA_update()`](https://scienceverse.github.io/metacheck/dev/reference/FLoRA_update.md)
  : Update FLoRA
- [`FLoRA_date()`](https://scienceverse.github.io/metacheck/dev/reference/FLoRA_date.md)
  : Get date FLoRA was updated
- [`openalex_doi()`](https://scienceverse.github.io/metacheck/dev/reference/openalex_doi.md)
  : OpenAlex info from DOI
- [`openalex_query()`](https://scienceverse.github.io/metacheck/dev/reference/openalex_query.md)
  : Look up a reference in OpenAlex
- [`pubpeer_comments()`](https://scienceverse.github.io/metacheck/dev/reference/pubpeer_comments.md)
  : Get Pubpeer Comments
- [`retractionwatch()`](https://scienceverse.github.io/metacheck/dev/reference/retractionwatch.md)
  [`rw()`](https://scienceverse.github.io/metacheck/dev/reference/retractionwatch.md)
  : RetractionWatch data
- [`rw_date()`](https://scienceverse.github.io/metacheck/dev/reference/rw_date.md)
  : Get date retractionwatch was updated
- [`rw_update()`](https://scienceverse.github.io/metacheck/dev/reference/rw_update.md)
  : Update retractionwatch

## OSF Functions

- [`osf_links()`](https://scienceverse.github.io/metacheck/dev/reference/osf_links.md)
  : Find OSF Links in Papers
- [`osf_info()`](https://scienceverse.github.io/metacheck/dev/reference/osf_info.md)
  : Retrieve info from the OSF by ID
- [`osf_file_download()`](https://scienceverse.github.io/metacheck/dev/reference/osf_file_download.md)
  : Download all OSF Project Files
- [`osf_api_check()`](https://scienceverse.github.io/metacheck/dev/reference/osf_api_check.md)
  : Check OSF API Server Status
- [`osf_check_id()`](https://scienceverse.github.io/metacheck/dev/reference/osf_check_id.md)
  : Check OSF IDs
- [`osf_delay()`](https://scienceverse.github.io/metacheck/dev/reference/osf_delay.md)
  : Set the OSF delay
- [`osf_get_all_pages()`](https://scienceverse.github.io/metacheck/dev/reference/osf_get_all_pages.md)
  : Get All OSF API Query Pages
- [`osf_preprint_list()`](https://scienceverse.github.io/metacheck/dev/reference/osf_preprint_list.md)
  : Get A list of preprints from the OSF
- [`osf_type()`](https://scienceverse.github.io/metacheck/dev/reference/osf_type.md)
  : Get OSF GUID Type

## GitHub Functions

- [`github_links()`](https://scienceverse.github.io/metacheck/dev/reference/github_links.md)
  : Find GitHub Links in Papers
- [`github_repo()`](https://scienceverse.github.io/metacheck/dev/reference/github_repo.md)
  : Get Short GitHub Repo Name
- [`github_info()`](https://scienceverse.github.io/metacheck/dev/reference/github_info.md)
  : Get GitHub Repo Info
- [`github_files()`](https://scienceverse.github.io/metacheck/dev/reference/github_files.md)
  : Get File List from GitHub
- [`github_readme()`](https://scienceverse.github.io/metacheck/dev/reference/github_readme.md)
  : Get README from GitHub
- [`github_languages()`](https://scienceverse.github.io/metacheck/dev/reference/github_languages.md)
  : Get Languages from GitHub Repo

## Archive Functions

- [`local_files()`](https://scienceverse.github.io/metacheck/dev/reference/local_files.md)
  : List Local Files
- [`aspredicted_links()`](https://scienceverse.github.io/metacheck/dev/reference/aspredicted_links.md)
  : Find AsPredicted Links in Papers
- [`aspredicted_info()`](https://scienceverse.github.io/metacheck/dev/reference/aspredicted_info.md)
  : Retrieve info from AsPredicted by URL
- [`rbox_links()`](https://scienceverse.github.io/metacheck/dev/reference/rbox_links.md)
  : Find ResearchBox Links in Papers
- [`rbox_info()`](https://scienceverse.github.io/metacheck/dev/reference/rbox_info.md)
  : Retrieve info from ResearchBox by URL
- [`rbox_file_download()`](https://scienceverse.github.io/metacheck/dev/reference/rbox_file_download.md)
  : Retrieve files from ResearchBox by URL
- [`zenodo_links()`](https://scienceverse.github.io/metacheck/dev/reference/zenodo_links.md)
  : Find Zenodo Links in Papers
- [`zenodo_info()`](https://scienceverse.github.io/metacheck/dev/reference/zenodo_info.md)
  : Retrieve info from Zenodo by URL
- [`zenodo_file_download()`](https://scienceverse.github.io/metacheck/dev/reference/zenodo_file_download.md)
  : Download all Zenodo Project Files

## Integrations

- [`email()`](https://scienceverse.github.io/metacheck/dev/reference/email.md)
  : Set or get email
- [`filetype()`](https://scienceverse.github.io/metacheck/dev/reference/filetype.md)
  : Get file Type from Extension
- [`file_category()`](https://scienceverse.github.io/metacheck/dev/reference/file_category.md)
  : Categorise files
- [`causal_relations()`](https://scienceverse.github.io/metacheck/dev/reference/causal_relations.md)
  : Extract causal relations from sentence(s) via a Hugging Face Space

## Validation Functions

- [`validate()`](https://scienceverse.github.io/metacheck/dev/reference/validate.md)
  : Validate
- [`accuracy()`](https://scienceverse.github.io/metacheck/dev/reference/accuracy.md)
  : Accuracy

## Extras

- [`extract_eq()`](https://scienceverse.github.io/metacheck/dev/reference/extract_eq.md)
  : Extract P-Values
- [`extract_p_values()`](https://scienceverse.github.io/metacheck/dev/reference/extract_p_values.md)
  : Extract P-Values
- [`extract_urls()`](https://scienceverse.github.io/metacheck/dev/reference/extract_urls.md)
  : Extract URLs
- [`stats()`](https://scienceverse.github.io/metacheck/dev/reference/stats.md)
  : Check Stats
- [`emojis`](https://scienceverse.github.io/metacheck/dev/reference/emojis.md)
  : Emojis
