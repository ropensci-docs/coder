# Convert output from classify() to matrix/data.frame/data.table

Convert output from classify() to matrix/data.frame/data.table

## Usage

``` r
# S3 method for class 'classified'
as.data.frame(x, ...)

# S3 method for class 'classified'
as.data.table(x, ...)

# S3 method for class 'classified'
as.matrix(x, ...)
```

## Arguments

- x:

  output from
  [`classify()`](https://docs.ropensci.org/coder/reference/classify.md)

- ...:

  ignored

## Value

data frame/data table with:

- first column named as "id" column specified as input to
  [`classify()`](https://docs.ropensci.org/coder/reference/classify.md)
  and with data from `row.names(x)`

- all columns from `classified`

- no row names

or simply the input matrix without additional attributes

## See also

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

## Examples

``` r
x <- classify(c("C80", "I20", "unvalid_code"), "elixhauser")
#> Classification based on: icd10

as.matrix(x)[, 1:3]
#>              congestive heart failure cardiac arrhythmias valvular disease
#> C80                             FALSE               FALSE            FALSE
#> I20                             FALSE               FALSE            FALSE
#> unvalid_code                    FALSE               FALSE            FALSE
as.data.frame(x)[, 1:3]
#>             id congestive heart failure cardiac arrhythmias
#> 1          C80                    FALSE               FALSE
#> 2          I20                    FALSE               FALSE
#> 3 unvalid_code                    FALSE               FALSE
data.table::as.data.table(x)[, 1:3]
#>              id congestive heart failure cardiac arrhythmias
#>          <char>                   <lgcl>              <lgcl>
#> 1:          C80                    FALSE               FALSE
#> 2:          I20                    FALSE               FALSE
#> 3: unvalid_code                    FALSE               FALSE

# `as_tibble()` works automatically due to internal use of `as.data.frame()`.
tibble::as_tibble(x)
#> # A tibble: 3 × 31
#>   `congestive heart failure` `cardiac arrhythmias` `valvular disease`
#>   <classifd>                 <classifd>            <classifd>        
#> 1 FALSE                      FALSE                 FALSE             
#> 2 FALSE                      FALSE                 FALSE             
#> 3 FALSE                      FALSE                 FALSE             
#> # ℹ 28 more variables: `pulmonary circulation disorder` <classifd>,
#> #   `peripheral vascular disorder` <classifd>,
#> #   `hypertension uncomplicated` <classifd>,
#> #   `hypertension complicated` <classifd>, paralysis <classifd>,
#> #   `other neurological disorders` <classifd>,
#> #   `chronic pulmonary disease` <classifd>,
#> #   `diabetes uncomplicated` <classifd>, `diabetes complicated` <classifd>, …
```
