# Classcodes methods

`classcodes` are classification schemes based on regular expression
stored in data frames. These are essential to the package and constitute
the third part of the triad of case data, code data and a classification
scheme.

## Usage

``` r
as.classcodes(x, ...)

# S3 method for class 'classcodes'
as.classcodes(
  x,
  ...,
  regex = attr(x, "regexpr"),
  indices = attr(x, "indices"),
  hierarchy = attr(x, "hierarchy")
)

# S3 method for class 'data.frame'
as.classcodes(
  x,
  ...,
  regex = NULL,
  indices = NULL,
  hierarchy = attr(x, "hierarchy"),
  .name = NULL
)

is.classcodes(x)
```

## Arguments

- x:

  data frame with columns described in the details section.
  Alternatively a `classcodes` object to be modified.

- ...:

  arguments passed between methods#'

- regex, indices:

  character vector with names of columns in `x` containing regular
  expressions/indices.

- hierarchy:

  named list of pairwise group names to appear as superior and
  subordinate for indices. To be used for indexing when the subordinate
  class is redundant (see the details section of
  [`elixhauser`](https://docs.ropensci.org/coder/reference/elixhauser.md)
  for an example).

- .name:

  used internally for name dispatch

## Value

Object of class `classcodes` (inheriting from data frame) with
additional attributes:

- `code:` the coding used (for example "icd10", or "ATC"). `NULL` for
  unknown/arbitrary coding.

- `regexprs:` name of columns with regular expressions (as specified by
  the `regex`argument)

- `indices:` name of columns with (optional) index weights (as specified
  by the `indices`argument)

- `hierarchy:` list as specified by the `hierarchy` argument.

- `name:` name as specified by the `.name` argument.

## Details

A classcodes object is a data frame with mandatory columns:

- `group`: unique and non missing class names

- At least one column with regular expressions
  ([regex](https://rdrr.io/r/base/regex.html) without Perl-like
  versions) defining class membership. Those columns can have arbitrary
  names (as specified by the `regex` argument). Occurrences of non
  unique regular expressions will lead to the same class having multiple
  names. This is accepted but will raise a warning. Classes do not have
  to be disjunct.

The object can have additional optional columns:

- `description`: description of each category

- `condition`: a class might have conditions additional to what is
  expressed by the regular expressions. If so, these should be specified
  as quoted expressions that can be evaluated within the data frame used
  by
  [`classify()`](https://docs.ropensci.org/coder/reference/classify.md)

- weights for each class used by
  [`index()`](https://docs.ropensci.org/coder/reference/index_fun.md).
  Could be more than one and could have arbitrary names (as specified by
  the `indices`argument).

## See also

[`vignette("classcodes")`](https://docs.ropensci.org/coder/articles/classcodes.md)
[`vignette("Interpret_regular_expressions")`](https://docs.ropensci.org/coder/articles/Interpret_regular_expressions.md)
The package have several default classcodes included, see
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md).

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

## Examples

``` r
# The Elixhauser comorbidity classification is already a classcodes object
is.classcodes(coder::elixhauser)
#> [1] TRUE

# Strip its class attributes to use in examples
df <- as.data.frame(coder::elixhauser)

# Specify which columns store regular expressions and indices
# (assume no hierarchy)
elix <-
  as.classcodes(
    df,
    regex     = c("icd10", "icd10_short", "icd9cm", "icd9cm_ahrqweb", "icd9cm_enhanced"),
    indices   = c("sum_all", "sum_all_ahrq", "walraven",
                "sid29", "sid30", "ahrq_mort", "ahrq_readm"),
    hierarchy = NULL
  )
elix
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd10, icd10_short, icd9cm, icd9cm_ahrqweb, icd9cm_enhanced 
#> Indices:
#>    sum_all, sum_all_ahrq, walraven, sid29, sid30, ahrq_mort, ahrq_readm   
#> 
#> # A tibble: 31 × 13
#>    group         icd10 icd10_short icd9cm icd9cm_ahrqweb icd9cm_enhanced sum_all
#>    <chr>         <chr> <chr>       <chr>  <chr>          <chr>             <dbl>
#>  1 congestive h… I(09… I(09|1[13]… 39891… 39891|4(0(2[0… 39891|4(0(2[01…       1
#>  2 cardiac arrh… I(44… I(4[457-9]… 42(6(… NA             42(6([079|1[02…       1
#>  3 valvular dis… A520… A52|I(0[5-… 0932|… 0932|39([4-6]… 0932|39[4-7]|4…       1
#>  4 pulmonary ci… I(2(… I2[678]     41(6|… 41(6|79)       41(5[01]|6|7[0…       1
#>  5 peripheral v… I7([… I7[01389]|… 44(0|… 44([0-2]|3[1-… 0930|4(373|4([…       1
#>  6 hypertension… I10   I10         401[1… 401[19]|6420   401                   1
#>  7 hypertension… I1[1… I1[1-35]    40([2… 40(10|[2-5])|… 40[2-5]               1
#>  8 paralysis     G(04… G(04|11|8[… 34(2[… 34[2-4]|438[2… 3(341|4([23]|4…       1
#>  9 other neurol… G(1[… G(1[0-3]|2… 3(3(1… 3(3([0145]|20… 3(3(19|2[01]|3…       1
#> 10 chronic pulm… (I27… I27|(J([46… 49(([… 49|50([0-5]|6… 4(16[89]|90)|5…       1
#> # ℹ 21 more rows
#> # ℹ 6 more variables: sum_all_ahrq <dbl>, walraven <dbl>, sid29 <dbl>,
#> #   sid30 <dbl>, ahrq_mort <dbl>, ahrq_readm <dbl>

# Specify hierarchy for patients with different types of cancer and diabetes
# See `?elixhauser` for details
as.classcodes(
  elix,
  hierarchy = list(
    cancer   = c("metastatic cancer", "solid tumor"),
    diabetes = c("diabetes complicated", "diabetes uncomplicated")
  )
)
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd10, icd10_short, icd9cm, icd9cm_ahrqweb, icd9cm_enhanced 
#> Indices:
#>    sum_all, sum_all_ahrq, walraven, sid29, sid30, ahrq_mort, ahrq_readm 
#> Hierarchy:
#>    c("metastatic cancer", "solid tumor"),
#>    c("diabetes complicated", "diabetes uncomplicated") 
#> 
#> # A tibble: 31 × 13
#>    group         icd10 icd10_short icd9cm icd9cm_ahrqweb icd9cm_enhanced sum_all
#>    <chr>         <chr> <chr>       <chr>  <chr>          <chr>             <dbl>
#>  1 congestive h… I(09… I(09|1[13]… 39891… 39891|4(0(2[0… 39891|4(0(2[01…       1
#>  2 cardiac arrh… I(44… I(4[457-9]… 42(6(… NA             42(6([079|1[02…       1
#>  3 valvular dis… A520… A52|I(0[5-… 0932|… 0932|39([4-6]… 0932|39[4-7]|4…       1
#>  4 pulmonary ci… I(2(… I2[678]     41(6|… 41(6|79)       41(5[01]|6|7[0…       1
#>  5 peripheral v… I7([… I7[01389]|… 44(0|… 44([0-2]|3[1-… 0930|4(373|4([…       1
#>  6 hypertension… I10   I10         401[1… 401[19]|6420   401                   1
#>  7 hypertension… I1[1… I1[1-35]    40([2… 40(10|[2-5])|… 40[2-5]               1
#>  8 paralysis     G(04… G(04|11|8[… 34(2[… 34[2-4]|438[2… 3(341|4([23]|4…       1
#>  9 other neurol… G(1[… G(1[0-3]|2… 3(3(1… 3(3([0145]|20… 3(3(19|2[01]|3…       1
#> 10 chronic pulm… (I27… I27|(J([46… 49(([… 49|50([0-5]|6… 4(16[89]|90)|5…       1
#> # ℹ 21 more rows
#> # ℹ 6 more variables: sum_all_ahrq <dbl>, walraven <dbl>, sid29 <dbl>,
#> #   sid30 <dbl>, ahrq_mort <dbl>, ahrq_readm <dbl>

# Several checks are performed to not allow any erroneous classcodes object
if (FALSE) { # \dontrun{
  as.classcodes(iris)
  as.classcodes(iris, regex = "Species")
} # }
```
