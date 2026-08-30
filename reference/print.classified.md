# Printing classified data

Preview first `n` rows as tibble

## Usage

``` r
# S3 method for class 'classified'
print(x, ...)
```

## Arguments

- x:

  output from
  [`classify()`](https://docs.ropensci.org/coder/reference/classify.md)

- ...:

  additional arguments passed to printing method for a `tibble`. `n` is
  the number of rows to preview. Set `n = NULL` to disable the `tibble`
  preview and print the object as is (a matrix).

## See also

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

## Examples

``` r
# Preview all output
classify(c("C80", "I20", "unvalid_code"), "elixhauser")
#> Classification based on: icd10
#> 
#> The printed data is of class: classified, matrix.
#> It has 3 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 3 × 31
#>   `congestive heart failure` `cardiac arrhythmias` `valvular disease`
#>   <lgl>                      <lgl>                 <lgl>             
#> 1 FALSE                      FALSE                 FALSE             
#> 2 FALSE                      FALSE                 FALSE             
#> 3 FALSE                      FALSE                 FALSE             
#> # ℹ 28 more variables: `pulmonary circulation disorder` <lgl>,
#> #   `peripheral vascular disorder` <lgl>, `hypertension uncomplicated` <lgl>,
#> #   `hypertension complicated` <lgl>, paralysis <lgl>,
#> #   `other neurological disorders` <lgl>, `chronic pulmonary disease` <lgl>,
#> #   `diabetes uncomplicated` <lgl>, `diabetes complicated` <lgl>,
#> #   hypothyroidism <lgl>, `renal failure` <lgl>, `liver disease` <lgl>,
#> #   `peptic ulcer disease` <lgl>, `AIDS/HIV` <lgl>, lymphoma <lgl>, …

# Preview only the first row
print(classify(c("C80", "I20", "unvalid_code"), "elixhauser"), n = 1)
#> Classification based on: icd10
#> 
#> The printed data is of class: classified, matrix.
#> It has 3 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 1 × 31
#>   `congestive heart failure` `cardiac arrhythmias` `valvular disease`
#>   <lgl>                      <lgl>                 <lgl>             
#> 1 FALSE                      FALSE                 FALSE             
#> # ℹ 28 more variables: `pulmonary circulation disorder` <lgl>,
#> #   `peripheral vascular disorder` <lgl>, `hypertension uncomplicated` <lgl>,
#> #   `hypertension complicated` <lgl>, paralysis <lgl>,
#> #   `other neurological disorders` <lgl>, `chronic pulmonary disease` <lgl>,
#> #   `diabetes uncomplicated` <lgl>, `diabetes complicated` <lgl>,
#> #   hypothyroidism <lgl>, `renal failure` <lgl>, `liver disease` <lgl>,
#> #   `peptic ulcer disease` <lgl>, `AIDS/HIV` <lgl>, lymphoma <lgl>, …

# Print object as is (matrix)
print(classify(c("C80", "I20", "unvalid_code"), "elixhauser"), n = NULL)
#> Classification based on: icd10
#> 
#> The printed data is of class: classified, matrix.
#> It has 3 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 3 × 31
#>   `congestive heart failure` `cardiac arrhythmias` `valvular disease`
#>   <lgl>                      <lgl>                 <lgl>             
#> 1 FALSE                      FALSE                 FALSE             
#> 2 FALSE                      FALSE                 FALSE             
#> 3 FALSE                      FALSE                 FALSE             
#> # ℹ 28 more variables: `pulmonary circulation disorder` <lgl>,
#> #   `peripheral vascular disorder` <lgl>, `hypertension uncomplicated` <lgl>,
#> #   `hypertension complicated` <lgl>, paralysis <lgl>,
#> #   `other neurological disorders` <lgl>, `chronic pulmonary disease` <lgl>,
#> #   `diabetes uncomplicated` <lgl>, `diabetes complicated` <lgl>,
#> #   hypothyroidism <lgl>, `renal failure` <lgl>, `liver disease` <lgl>,
#> #   `peptic ulcer disease` <lgl>, `AIDS/HIV` <lgl>, lymphoma <lgl>, …
```
