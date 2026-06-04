# Pluralise

Helper function for conditional plurals. For example, if you want to
return "1 error" or "2 errors", you can use this in a sprintf().

## Usage

``` r
plural(n, singular = "", plural = "s")
```

## Arguments

- n:

  the number

- singular:

  the word or ending when n = 1

- plural:

  the word or ending n != 1

## Value

a string

## Examples

``` r
n <- 0:3
sprintf("I have %d friend%s", n, plural(n))
#> [1] "I have 0 friends" "I have 1 friend"  "I have 2 friends" "I have 3 friends"
sprintf("I have %d %s", n, plural(n, "octopus", "octopi"))
#> [1] "I have 0 octopi"  "I have 1 octopus" "I have 2 octopi"  "I have 3 octopi" 
```
