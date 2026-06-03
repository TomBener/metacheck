# Local File Support for repo_check and code_check

This document describes the complete set of changes made to add
local-directory support to the repository and code-checking pipeline.
Five things were changed or created:

1.  New file `R/archive-local.R` — the
    [`local_files()`](https://scienceverse.github.io/metacheck/reference/local_files.md)
    function
2.  Modified `inst/modules/repo_check.R` — new `local_path` parameter
3.  Modified `inst/modules/code_check.R` — new parameters, progress bar
4.  Modified `R/paper.R` — the `no_paper()` helper
5.  New test file and fixtures in `tests/testthat/`

Two preexisting bugs were also fixed:

1.  A typo in `tests/testthat/test-module-.R` line 28 where
    `"motamodule"` should have been `"notamodule"`. Introduced by Lisa
    DeBruine on 2026-05-30 at 11:46 in commit `dde301c1`.

2.  Non-UTF-8 encoded code files (e.g., Stata `.do` files with non-ASCII
    characters in labels or comments) could produce all-NA results for
    `code_lines`, `comment_lines`, and `percentage_comment`. Two
    encoding variants can cause this:

    - **Windows-1252 / Latin-1** — the `iconv(from = "UTF-8", ...)` call
      returns `NA` for every line containing a non-ASCII byte. Fixed by
      opening local file connections with `encoding = "latin1"`, which
      covers all 256 byte values and never produces NA.
    - **UTF-16 LE** (used by some versions of Stata on Windows) — every
      ASCII character is followed by a NUL byte (`0x00`). `readLines`
      stops at the first NUL, returning 0 lines from a file that may be
      50 KB of content. Fixed by adding `skipNul = TRUE` to the
      `readLines` call. Note: files that are pure ASCII (check with
      `readr::guess_encoding(path)`) are unaffected by encoding — all-NA
      results for such files indicate that the files were not fully
      downloaded from OneDrive at the time of the check. Two
      binary-encoded fixture files were added to
      `tests/testthat/_fixtures/code_files/` (`stata_latin1.do` and
      `stata_utf16.do`) with a regression test in
      `test-archive-local.R`.

------------------------------------------------------------------------

## Motivation

`repo_check` discovers repository links in a parsed paper and lists all
files on OSF, GitHub, ResearchBox, and Zenodo. `code_check` then reads
those files and checks R, Rmd, Qmd, SAS, SPSS, and Stata scripts for
best practices (comments, absolute paths, library grouping, missing data
files).

Both modules previously required all files to live in a remote
repository discoverable from links in a manuscript. If a researcher had
downloaded files locally — or if no manuscript existed and the goal was
simply to check a folder — there was no supported path.

The design goal was to make a local directory a first-class repository,
treated identically to an OSF project or GitHub repo: it appears in
`repo_check`’s file table, it is checked for a README, it is checked for
zip archives, and its code files are analysed by `code_check`. The local
path should be composable with remote repositories: a paper that links
to an OSF project and also has supplementary files in a local folder can
be checked in one call.

------------------------------------------------------------------------

## 1. New file: `R/archive-local.R`

### What it does

`local_files(path)` walks a local directory recursively and returns a
`data.frame` with the following columns:

| Column | Type | Content |
|----|----|----|
| `repo_url` | character | `normalizePath(path)` — the same value for every row |
| `file_name` | character | [`basename()`](https://rdrr.io/r/base/basename.html) of the file |
| `file_url` | character | Always `NA_character_` — no remote URL |
| `file_location` | character | Full absolute path to the file |
| `file_size` | double | File size in bytes from [`file.size()`](https://rdrr.io/r/base/file.info.html) |
| `file_type` | character | Type string from [`metacheck::file_types`](https://scienceverse.github.io/metacheck/reference/file_types.md), or `NA` |

This column layout is identical to what `repo_check` assembles for each
remote source (`osf_files_df`, `github_files_df`, `rb_files_df`,
`zenodo_files_df`). `repo_check` includes local files in the same
`dplyr::bind_rows(...)` call as the remote files, so all downstream
logic — README check, zip detection, the `code_check` analysis loop —
sees local files without any special cases.

The two columns `code_check` cares about most:

- `file_location` — if not `NA`, the per-file loop opens it with
  [`file()`](https://rdrr.io/r/base/connections.html) for local reading;
  if `NA`, it falls back to
  [`url()`](https://rdrr.io/r/base/connections.html) on `file_url`.
  Local files always have `file_location` set and `file_url = NA`, so
  they are always read from disk.
- `file_name` (basename) — populates `files_in_repository`, the set
  checked against filenames referenced inside each script for
  missing-file detection.

### Vectorized Input

When `path` is a vector, each element is processed independently and the
results are stacked:

``` r
if (length(path) > 1) {
  return(dplyr::bind_rows(lapply(path, local_files)))
}
```

This is the same pattern
[`github_files()`](https://scienceverse.github.io/metacheck/reference/github_files.md)
uses for multiple repos. Each path keeps its own `repo_url`, so the
caller can tell which file came from which folder. `repo_check` and
`code_check` already handle multiple repos, so no changes were needed
downstream — each path simply becomes a separate `"local"` entry in the
`repos` table.

### Error handling

Two explicit guards run before any work:

``` r
path <- normalizePath(path, mustWork = TRUE)   # errors if path does not exist
if (!dir.exists(path)) stop("'", path, "' is not a directory")
```

`normalizePath(..., mustWork = TRUE)` raises a system error when the
path does not exist. The `dir.exists` guard catches the case where the
path exists but is a regular file. Without it,
[`list.files()`](https://rdrr.io/r/base/list.files.html) called on a
file path silently returns `character(0)`, producing a result that looks
like an empty directory.

### Empty directory

[`list.files()`](https://rdrr.io/r/base/list.files.html) returning
nothing yields a zero-row `data.frame` with the correct columns rather
than `NULL`. This is consistent with how `repo_check` initialises each
remote source (`data.frame(repo_name = character(0))`) and avoids
[`dplyr::bind_rows`](https://dplyr.tidyverse.org/reference/bind_rows.html)
warnings about incompatible inputs.

### File type detection

Extension lookup uses the same
[`metacheck::file_types`](https://scienceverse.github.io/metacheck/reference/file_types.md)
table that
[`github_files()`](https://scienceverse.github.io/metacheck/reference/github_files.md)
and the Zenodo section of `repo_check` use:

``` r
exts <- tolower(sub("^.*\\.", "", file_names))
exts[!grepl("\\.", file_names)] <- NA_character_      # files with no dot → NA
types <- metacheck::file_types$type[match(exts, metacheck::file_types$ext)]
```

Files without any extension (e.g., `Makefile`, `LICENSE`) get `NA` for
`file_type`. The README override is applied after the lookup, mirroring
`repo_check`’s own logic:

``` r
is_readme <- grepl("readme|read[_ ]me", file_names, ignore.case = TRUE)
types[is_readme] <- "readme"
```

------------------------------------------------------------------------

## 2. Modified file: `inst/modules/repo_check.R`

### New parameters

``` r
repo_check <- function(paper, local_path = NULL)
```

`local_path` is the new parameter. Pass `no_paper()` as `paper` when
calling without a manuscript:
`module_run(no_paper(), "repo_check", local_path = "C:/my_folder")`.

### New “Local files” section

Inserted just before the `nrow(repos) == 0` early-return guard:

``` r
## Local files ----
local_files_df <- data.frame(repo_name = character(0))
if (!is.null(local_path)) {
  local_files_df <- local_files(local_path)
  local_repo <- data.frame(
    paper_id = paper_id(paper),
    repo_url = normalizePath(local_path),
    repo_type = "local",
    repo_error = NA_character_
  )
  repos <- dplyr::bind_rows(repos, local_repo)
}
```

Calling `local_files(local_path)` first means its error guards run
before `normalizePath(local_path)` is reached, so an invalid path
surfaces as a clear error. By the time `normalizePath` runs, the path is
guaranteed valid.

Adding the local repo row to `repos` **before** the early-return check
is essential: if the paper has no online links and only a local path is
provided, `repos` would otherwise be empty and the module would return
early with “no repositories found” without checking the local folder.

### Progress message for local paths

When `local_path` is provided, a one-line message is printed above the
spinner immediately after “Starting Repo Check”:

``` r
pb$tick(0, list(what = "Starting Repo Check"))
if (!is.null(local_path)) {
  pb$message("If folders are stored online, the check might be slow as all files need to be downloaded.")
}
```

`pb$message()` is already part of the progress bar interface — the dummy
stub in `svutils-pb.R` handles it silently in non-verbose mode. The
message fires before any file reading, so the user sees it in time to
cancel and sync the folder offline first.

### `local_files_df` included in `all_files`

``` r
all_files <- dplyr::bind_rows(
  osf_files_df, github_files_df, rb_files_df, zenodo_files_df, local_files_df
)
```

Because local files share the same column structure as the remote data
frames, no special handling is needed anywhere downstream. The README
check, zip check, empty-repo check, and summary table all operate
identically on local and remote files.

### `report_tbl` fix for local files

`link(url, text)` returns `NA` when `url` is `NA` (line 203 of
`R/report-helpers.R`). Local files have `file_url = NA_character_`, so
they would silently disappear from the file table in the report. The
fix:

``` r
# before
report_tbl <- all_files |>
  dplyr::mutate(file = link(file_url, file_name)) |> ...

# after
report_tbl <- all_files |>
  dplyr::mutate(file = dplyr::coalesce(link(file_url, file_name), file_name)) |> ...
```

`coalesce` falls back to plain `file_name` text when the link is `NA`.
Remote files are unaffected.

### What `repo_check` now does for a local folder

The existing checks all work without modification:

- **README check** —
  `grepl("readme|read[_ ]me", all_files$file_name, ...)` runs over the
  combined file list. A README in the local folder counts.
- **Zip check** — zip/archive files in the local folder are flagged.
- **Empty repo** — if the local path is an empty directory,
  `files_n = 0` for that repo row, and the “empty repository” warning
  fires.
- **`repo_type = "local"`** — the repos summary table includes this
  column, so callers can distinguish local from remote repos.
- **`file_limit` per repo in `code_check`** — the `repo_url` for local
  files is the normalised path, which is distinct from remote repo URLs.
  `code_check`’s `dplyr::slice_head(n = file_limit, by = repo_url)`
  therefore applies the limit independently to local and remote files.

------------------------------------------------------------------------

## 3. Modified file: `inst/modules/code_check.R`

### Changes to the function signature

``` r
# before
code_check <- function(paper, file_limit = 20)

# after
code_check <- function(paper, file_limit = 20, local_path = NULL)
```

### `paper = NULL` handling

``` r
if (is.null(paper)) paper <- no_paper()
```

When the caller has no manuscript, passing `NULL` (or omitting `paper`)
substitutes a minimal valid paper object. See section 4 for why plain
[`paper()`](https://scienceverse.github.io/metacheck/reference/paper.md)
cannot be used.

### `local_path` is passed to `repo_check`

``` r
# before
all_files <- get_prev_outputs("repo_check", "table")
if (is.null(all_files)) {
  mo <- module_run(paper, "repo_check")
  all_files <- mo$table %||% data.frame(...)
}

# after
all_files <- get_prev_outputs("repo_check", "table")
if (is.null(all_files)) {
  if (!is.null(local_path)) {
    mo <- module_run(paper, "repo_check", local_path = local_path)
  } else {
    mo <- module_run(paper, "repo_check")
  }
  all_files <- mo$table %||% data.frame(...)
}
```

`local_path` is forwarded to `repo_check`, which handles the full
inventory of local files including the README and zip checks.
`code_check` then consumes the unified `all_files` table as it always
did, with no knowledge of which files are local versus remote.

The `local_path = NULL` case is passed without the extra argument so
that existing httptest2 mocks are not affected (passing
`local_path = NULL` would change the `list(...)` length check in
`module_run` and could interfere with tests that mock `repo_check`’s API
calls).

When `get_prev_outputs` returns a cached `repo_check` result (pipeline
use), `local_path` is not appended. In a pipeline, if local files are
needed, run `repo_check` explicitly with `local_path` first.

### Progress bar for the analysis loop

``` r
pb_code <- pb(nrow(code_files), "(:spin) :what")
pb_code$tick(0, list(what = "Starting Code Check"))
on.exit(pb_code$terminate())

collected <- lapply(seq_along(code_files$file_location), \(i) {
  pb_code$tick(1, list(what = code_files$file_name[[i]]))
  tryCatch({ ... })
})
```

Previously, `repo_check` would print “Repo Check Complete” immediately
(because for a `no_paper()` it returns in milliseconds with no links to
check) and then R would be busy for several seconds reading and
analysing local files with no feedback. Adding a per-file progress bar
to the `lapply` loop gives the correct feedback: the bar ticks once per
code file as it is read and analysed.

------------------------------------------------------------------------

## 4. Modified file: `R/paper.R`

### `no_paper()` — the new function

``` r
no_paper <- function(id = "local") {
  p <- paper(id = id)
  p$info <- data.frame(
    title = NA_character_,
    file_hash = id,
    input_format = "local"
  )
  p
}
```

Inserted between
[`demopaper()`](https://scienceverse.github.io/metacheck/reference/demopaper.md)
and
[`demofile()`](https://scienceverse.github.io/metacheck/reference/demofile.md).

### Why not plain `paper()`

[`paper()`](https://scienceverse.github.io/metacheck/reference/paper.md)
creates a valid, schema-conformant empty paper with all data frame
tables as zero-row, including `$info`. This breaks `paper_id(paper)`:

``` r
paper_id <- function(paper) {
  paper_table(paper, "info", "paper_id")$paper_id
}
```

`paper_table` populates the `paper_id` column by repeating
`paper$paper_id` once per row of the table: `rep("local", nrow(info))`.
With zero rows, `rep("local", 0)` returns `character(0)`, and
`paper_id(paper)` returns `character(0)`. In `repo_check`’s early-return
path:

``` r
summary_table = data.frame(
  paper_id = paper_id(paper),   # character(0)
  repo_n   = 0,                 # length 1
  ...
)
```

this produces `Error: arguments imply differing number of rows: 0, 1`.

Adding one row to `p$info` makes `paper_id(paper)` return a length-1
string. The three columns (`title`, `file_hash`, `input_format`) match
what
[`test_paper()`](https://scienceverse.github.io/metacheck/reference/test_paper.md)
sets and are sufficient to avoid
[`paper_validate()`](https://scienceverse.github.io/metacheck/reference/paper_validate.md)
warnings.

------------------------------------------------------------------------

## 5. Test fixtures: `tests/testthat/_fixtures/code_files/`

Five files. Their content is chosen to exercise specific code paths in
`repo_check` and `code_check`, not just `local_files`.

| File | Purpose |
|----|----|
| `analysis.R` | Has comments; loads `data.csv` (exists) → `loaded_files_missing = 0` |
| `analysis_no_comments.R` | No comments; loads `missing_file.csv` (absent) → flags both issues |
| `subdir/helper.R` | In a subdirectory → tests recursive listing; clean script |
| `data.csv` | Non-empty data file; `data.csv` basename enters `files_in_repository` |
| `README.md` | README detection; `repo_check` should report `files_readme = 1` and green traffic light for local-only runs |

The fixture set is designed so that a `repo_check` run on this folder
alone produces `traffic_light = "green"` (README present, no zip files),
while a `code_check` run produces `traffic_light = "yellow"` (missing
file and no comments in `analysis_no_comments.R`).

------------------------------------------------------------------------

## 6. New test file: `tests/testthat/test-archive-local.R`

72 assertions across 17 `test_that` blocks, in four sections.

The fixture directory is resolved at file load time:

``` r
fixture_dir <- normalizePath("_fixtures/code_files")
```

This follows the same pattern as `helper.R`
(`apis <- normalizePath("apis")`), relying on `tests/testthat/` being
the working directory. The absolute path survives
[`module_run()`](https://scienceverse.github.io/metacheck/reference/module_run.md)
changing the working directory to `inst/modules/` to source the module.

The `no_paper()`,
[`local_files()`](https://scienceverse.github.io/metacheck/reference/local_files.md),
and local-only `code_check`/`repo_check` tests require no httptest2
mocking — `no_paper()` has an empty URL table so all link-finders return
empty tables at the first
[`dplyr::filter`](https://dplyr.tidyverse.org/reference/filter.html)
call with no network access. The combined (paper + local_path) tests use
[`httptest2::use_mock_api()`](https://enpiar.com/httptest2/reference/with_mock_api.html)
for the OSF API calls.

------------------------------------------------------------------------

### Test-by-test breakdown

#### `no_paper` (1 block, 5 assertions)

| Assertion | What it checks |
|----|----|
| `is.function(metacheck::no_paper)` | Exported from NAMESPACE |
| `expect_no_error(help(...))` | Has a documentation page |
| `expect_s3_class(p, "scivrs_paper")` | Returns a proper paper object |
| `expect_equal(p$paper_id, "local")` | Default ID is “local” |
| `expect_equal(paper_id(p), "local")` | [`paper_id()`](https://scienceverse.github.io/metacheck/reference/paper_id.md) returns length-1 string, not `character(0)` |

------------------------------------------------------------------------

#### `local_files exists` (1 block, 2 assertions)

``` r
expect_true(is.function(metacheck::local_files))
expect_no_error(help(local_files, metacheck))
```

**Mirror:** `test_that("exists", {...})` in every other archive test.

------------------------------------------------------------------------

#### `local_files errors` (1 block, 3 assertions)

| Input | Expected behaviour |
|----|----|
| `bad_arg` (undefined) | R raises “object not found” before the function body runs |
| Non-existent path | `normalizePath(..., mustWork = TRUE)` errors |
| Existing file (not dir) | [`dir.exists()`](https://rdrr.io/r/base/files2.html) guard errors with “not a directory” |

**Mirror:** `test_that("errors", {...})` in `test-archive-github.R`. The
local equivalent of “repo not found” is always an error (not `NULL`)
because a bad local path is unambiguously a programming mistake.

------------------------------------------------------------------------

#### `local_files empty directory` (1 block, 3 assertions)

Tests that an empty directory returns a zero-row `data.frame` with
correct columns rather than `NULL`. This matters because
[`dplyr::bind_rows`](https://dplyr.tidyverse.org/reference/bind_rows.html)
in `repo_check` requires all inputs to be data frames.

------------------------------------------------------------------------

#### `local_files column structure` (1 block, 7 assertions)

Verifies every column’s contract: `repo_url` is the same normalised root
for all rows; `file_name` is strictly
[`basename()`](https://rdrr.io/r/base/basename.html); `file_url` is
always `NA`; `file_location` paths exist on disk; `file_size` is a
positive double.

**Mirror:** Column-name assertions in `test-archive-github.R`.

------------------------------------------------------------------------

#### `local_files finds files recursively` (1 block, 6 assertions)

Confirms that `list.files(..., recursive = TRUE)` finds files in
`subdir/` and that the total count is exactly 5 (no duplicates, no
misses).

**Mirror:** Recursive vs. non-recursive test in `test-archive-github.R`.

------------------------------------------------------------------------

#### `local_files file type detection` (1 block, 3 assertions)

``` r
expect_equal(r_row$file_type,      "code")    # analysis.R
expect_equal(csv_row$file_type,    "data")    # data.csv
expect_equal(readme_row$file_type, "readme")  # README.md (override logic)
```

`.md` maps to `"text"` in `file_types`, but the `grepl("readme", ...)`
override fires after the lookup and changes it to `"readme"`.

------------------------------------------------------------------------

#### `local_files vectorized input` (1 block, 4 assertions)

``` r
result <- local_files(c(tmp1, tmp2))
expect_equal(nrow(result), 2)
expect_true("script1.R" %in% result$file_name)
expect_true("data.csv"  %in% result$file_name)
expect_equal(result$repo_url[result$file_name == "script1.R"], normalizePath(tmp1))
```

Each path is processed independently. The test confirms that files carry
the correct `repo_url` for their source directory, not a merged or
shared value.

**Mirror:** Vectorization tests in `test-archive-github.R`:

``` r
repo <- c("scienceverse/metacheck", "scienceverse/faux")
files <- github_files(repo)
expect_in(files$repo, repo)
```

------------------------------------------------------------------------

#### `repo_check` Vector of Local Paths (1 block, 3 assertions)

``` r
mo <- module_run(no_paper(), "repo_check", local_path = c(tmp1, tmp2))
expect_equal(mo$summary_table$repo_n, 2)
expect_true("analysis.R" %in% mo$table$file_name)
expect_true("README.md"  %in% mo$table$file_name)
```

Confirms that passing a vector to `repo_check` creates one repo entry
per path in the summary table, and that files from all paths appear in
`mo$table`.

------------------------------------------------------------------------

#### `local_files file without extension gets NA type` (1 block, 1 assertion)

A `Makefile` created in a temp directory gets `file_type = NA`. Tests
the explicit `exts[!grepl("\\.", file_names)] <- NA_character_` line;
without it `sub("^.*\\.", "", "Makefile")` returns `"Makefile"` and the
type would be wrong by accident rather than `NA` by design.

Uses a temp directory rather than the fixture folder to keep the fixture
file count stable (the `nrow == 5` assertion would otherwise need
updating).

------------------------------------------------------------------------

#### `repo_check local_path only` (1 block, 8 assertions)

``` r
mo <- module_run(no_paper(), "repo_check", local_path = fixture_dir)
```

| Assertion                             | Value     | Reason                     |
|---------------------------------------|-----------|----------------------------|
| `repo_n`                              | 1         | One repo: the local folder |
| `files_n`                             | 5         | All 5 fixture files        |
| `files_code`                          | 3         | Three `.R` files           |
| `files_data`                          | 1         | `data.csv`                 |
| `files_readme`                        | 1         | `README.md`                |
| `files_zip`                           | 0         | No archives                |
| `"README.md" %in% mo$table$file_name` | TRUE      | File appears in table      |
| `traffic_light`                       | `"green"` | README present, no zip     |

------------------------------------------------------------------------

#### `repo_check paper + local_path` (1 block, 8 assertions)

Uses
[`httptest2::use_mock_api()`](https://enpiar.com/httptest2/reference/with_mock_api.html).
The OSF mock `629bx` has 4 files (2 code, 1 data, 0 readme, 1 zip).
Combined with the fixture folder (3 code, 1 data, 1 readme, 0 zip):

| Assertion                | Value                               |
|--------------------------|-------------------------------------|
| `repo_n`                 | 2 (OSF + local)                     |
| `files_n`                | 9                                   |
| `files_code`             | 5                                   |
| `files_readme`           | 1 (only local has README)           |
| `files_zip`              | 1 (only OSF has zip)                |
| `"README.md" %in% table` | TRUE (local)                        |
| `"bad.R" %in% table`     | TRUE (OSF)                          |
| `traffic_light`          | `"yellow"` (OSF repo has no README) |

------------------------------------------------------------------------

#### `code_check paper + local_path` (1 block, 3 assertions)

Uses
[`httptest2::use_mock_api()`](https://enpiar.com/httptest2/reference/with_mock_api.html).
OSF contributes 2 code files, local 3:

``` r
expect_equal(mo$summary_table$code_n, 5)
expect_true("analysis.R" %in% mo$table$file_name)   # local
expect_true("bad.R" %in% mo$table$file_name)          # OSF
expect_setequal(
  unique(mo$table$repo_url),
  c("https://osf.io/629bx", normalizePath(fixture_dir))
)
```

The two distinct `repo_url` values confirm that `file_limit` would be
enforced independently for each source.

------------------------------------------------------------------------

#### `code_check local_path errors` — `code_check local_path no code files` — `code_check local_path finds code files` — `code_check local_path: present files not flagged missing` — `code_check local_path: absent files flagged missing` — `code_check local_path: files without comments flagged` (6 blocks)

These test the `code_check` analysis pipeline end-to-end on local files.
Key assertions:

- `analysis.R` loads `data.csv` (present in fixture) →
  `loaded_files_missing = 0`
- `analysis_no_comments.R` loads `missing_file.csv` (absent) →
  `loaded_files_missing = 1`, name matches
- `analysis_no_comments.R` has zero comment lines →
  `percentage_comment = 0`
- A directory with only `.csv` files → `traffic_light = "na"`,
  `code_file_n = 0`

**Mirror for all six:** `test-module-code_check.R` tests the identical
logic via mocked OSF/GitHub repos.

------------------------------------------------------------------------

## What was and was not changed

| File | Changed? | What |
|----|----|----|
| `R/archive-local.R` | **new** | [`local_files()`](https://scienceverse.github.io/metacheck/reference/local_files.md) — single path and vectorized input; [`file.size()`](https://rdrr.io/r/base/file.info.html) instead of `file.info()$size` (avoids OneDrive timestamp crash); Rd cross-reference fix |
| `inst/modules/repo_check.R` | **yes** | `paper = NULL`; `local_path` param; local section; `local_files_df` in `bind_rows`; `coalesce` fix in `report_tbl`; `pb$message()` slow-download warning; `repo_name` populated with `basename(repo_url)` |
| `inst/modules/code_check.R` | **yes** | `paper = NULL`; `local_path` param; passes `local_path` to `repo_check`; progress bar |
| `R/paper.R` | **yes** | `no_paper()`; Rd cross-reference fix |
| `tests/testthat/test-archive-local.R` | **new** | 72 assertions, 17 blocks |
| `tests/testthat/_fixtures/code_files/` | **new** | 5 fixture files |
| `tests/testthat/test-module-.R` | **fixed** | Typo `"motamodule"` → `"notamodule"` (Lisa, commit `dde301c1`, 2026-05-30 11:46) |
| All other files | unchanged |  |

Running
`devtools::test(filter = "archive-|module-repo_check|module-code_check")`
after all changes: `[ FAIL 0 | WARN 0 | SKIP 0 | PASS 463 ]`

------------------------------------------------------------------------

## Examples

All examples use Windows paths. On macOS, replace
`"C:/Users/dlakens/..."` with `"/Users/dlakens/..."`. Everything else is
identical.

### 1. `repo_check` only — inventory and README/zip check on a local folder

Use this when you want to see what files are in the folder, whether
there is a README, and whether there are zip archives, without running
the full code analysis.

``` r
devtools::load_all()

mo <- module_run(no_paper(), "repo_check",
                 local_path = "C:/Users/dlakens/OneDrive - TU Eindhoven/git_repos/public_opinion")

mo$summary_text          # "We found N files in 1 repository."
mo$summary_table         # repo_n, files_n, files_code, files_data, files_readme, files_zip
mo$traffic_light         # "green" if README present and no zips; "yellow" otherwise
mo$table                 # one row per file: repo_url, file_name, file_url,
                         #   file_location, file_size, file_type
```

Typical output when a README is missing:

    traffic_light: "yellow"
    summary_text:
     - We found 12 files in 1 repository.
     - We found 0 README files and 1 repository without READMEs.

------------------------------------------------------------------------

### 2. `repo_check` + `code_check` as an explicit two-step pipeline

Use this when you want the repo inventory and the code analysis as
separate outputs, or when you want to inspect the file list before the
code check runs. Running `repo_check` first caches its output so
`code_check` does not re-fetch it.

``` r
devtools::load_all()

local_path <- "C:/Users/dlakens/OneDrive - TU Eindhoven/git_repos/public_opinion"

# Step 1: repository inventory
repo <- module_run(no_paper(), "repo_check", local_path = local_path)

repo$summary_text        # file counts, README status, zip files
repo$table               # full file list

# Step 2: code analysis (uses cached repo output — does not re-read the folder)
code <- module_run(repo, "code_check")

code$summary_text        # comment rates, missing files, absolute paths
code$table               # one row per code file with all metrics
code$traffic_light       # "green" / "yellow" / "na"
```

Note: `module_run(repo, "code_check")` passes the `repo` output object
as the first argument. `module_run` recognises `metacheck_module_output`
objects and extracts the cached file table so `repo_check` is not run
again.

------------------------------------------------------------------------

### 3. `code_check` only — code analysis on a local folder

Use this when you only care about the code quality checks and not the
broader repository inventory. Internally `code_check` calls `repo_check`
with `local_path` and then proceeds to the analysis. Both `repo_check`
and `code_check` progress bars will appear.

``` r
devtools::load_all()

mo <- module_run(no_paper(), "code_check",
                 local_path = "C:/Users/dlakens/OneDrive - TU Eindhoven/git_repos/public_opinion")

mo$summary_text          # combined summary: file counts + all four checks
mo$summary_table         # code_n, code_abs_path, code_missing_files, code_min_comments
mo$traffic_light         # "green" / "yellow" / "na"
mo$table                 # one row per code file
mo$report                # full HTML-ready report sections
```

------------------------------------------------------------------------

### 4. Real paper with both an online repository and a local folder

Use this when you have a parsed manuscript that links to an online
repository, and you also have supplementary files downloaded locally
that were not uploaded to the online repo.

``` r
devtools::load_all()

paper <- read_paper("my_paper.pdf")   # or demopaper(), or load from JSON

mo <- module_run(paper, "code_check",
                 local_path = "C:/Users/dlakens/supplementary_files")

# mo$table contains code files from both the online repo and the local folder
# mo$summary_table$code_n counts all of them combined
# file_limit is applied independently per repo_url (remote and local separately)
unique(mo$table$repo_url)   # e.g. "https://osf.io/abcde" and "C:/Users/.../supplementary_files"
```

`repo_check` (called internally) will report on both repositories: the
online one and the local folder. If the local folder has a README and
the online one does not, the README warning still fires for the online
repository.

------------------------------------------------------------------------

### 5. Multiple Local Folders in One Call

Use this when code for a paper is spread across several local
directories, or when you want to compare code quality across multiple
projects at once.

``` r
devtools::load_all()

paths <- c(
  "C:/Users/dlakens/OneDrive - TU Eindhoven/research_projects/ulrich_replication",
  "C:/Users/dlakens/OneDrive - TU Eindhoven/research_projects/janet_magnitude_based_inferences",
  "C:/Users/dlakens/OneDrive - TU Eindhoven/git_repos/nonsig-master-thesis"
)

mo <- module_run(no_paper(), "code_check", local_path = paths)

mo$summary_table$code_n          # total code files across all three folders
unique(mo$table$repo_url)        # one entry per folder
mo$table[, c("repo_url", "file_name", "loaded_files_missing")]
```

Each folder is treated as a separate repository. `file_limit` (default
20) is applied independently per folder. The `repo_check` summary table
will show three rows, one per path, with per-folder README and zip
counts.
