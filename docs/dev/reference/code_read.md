# Read code from files

Read code from files

## Usage

``` r
code_read(file_path)
```

## Arguments

- file_path:

  a file path or url to read in

## Value

a character vector of the file contents

## Examples

``` r
file_path <- demofile("json")
text <- code_read(file_path)
```
