# Write paper

Save a paper as a JSON file.

## Usage

``` r
paper_write(paper, file_name = NULL, save_path = ".")
```

## Arguments

- paper:

  a paper object

- file_name:

  the name of the file (if NULL, defaults to the paper_id)

- save_path:

  the directory to save the JSON file in

## Value

the path to the JSON file

## Examples

``` r
if (FALSE) { # \dontrun{
paper <- demopaper()
paper$info$title <- "New title"
paper_write(paper, "new_paper")
} # }
```
