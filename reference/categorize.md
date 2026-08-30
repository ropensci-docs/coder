# Categorize cases based on external data and classification scheme

This is the main function of the package, which relies of a triad of
objects: (1) `data` with unit id:s and possible dates of interest; (2)
`codedata` for corresponding units and with optional dates of interest
and; (3) a classification scheme
([`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md)
object; `cc`) with regular expressions to identify and categorize
relevant codes. The function combines the three underlying steps
performed by
[`codify()`](https://docs.ropensci.org/coder/reference/codify.md),
[`classify()`](https://docs.ropensci.org/coder/reference/classify.md)
and [`index()`](https://docs.ropensci.org/coder/reference/index_fun.md).
Relevant arguments are passed to those functions by `codify_args` and
`cc_args`.

## Usage

``` r
categorize(x, ...)

# S3 method for class 'data.frame'
categorize(x, ...)

# S3 method for class 'tbl_df'
categorize(x, ...)

# S3 method for class 'data.table'
categorize(x, ..., codedata, id, code, codify_args = list())

# S3 method for class 'codified'
categorize(
  x,
  ...,
  cc,
  index = NULL,
  cc_args = list(),
  check.names = TRUE,
  .data_cols = NULL
)
```

## Arguments

- x:

  data set with mandatory character id column (identified by argument
  `id = "<col_name>"`), and optional
  [`Date`](https://rdrr.io/r/base/Dates.html) of interest (identified by
  argument `date = "<col_name>"`). Alternatively, the output from
  [`codify()`](https://docs.ropensci.org/coder/reference/codify.md)

- ...:

  arguments passed between methods

- codedata:

  external code data with mandatory character id column (identified by
  `id = "<col_name>"`), code column (identified by argument
  `code = "<col_name>"`) and optional
  [`Date`](https://rdrr.io/r/base/Dates.html) column (identified by
  `codify_args = list(code_date = "<col_name>")`).

- id:

  name of unique character id column found in both `x`and `codedata`.
  (where it must not be unique).

- code:

  name of code column in `codedata`.

- codify_args:

  Lists of named arguments passed to
  [`codify()`](https://docs.ropensci.org/coder/reference/codify.md)

- cc:

  [`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md)
  object (or name of a default object from
  [`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md)).

- index:

  Argument passed to
  [`index()`](https://docs.ropensci.org/coder/reference/index_fun.md). A
  character vector of names of columns with index weights from the
  corresponding classcodes object (as supplied by the `cc`argument). See
  `attr(cc, "indices")` for available options. Set to `FALSE` if no
  index should be calculated. If `NULL`, the default, all available
  indices (from `attr(cc, "indices")`) are provided.

- cc_args:

  List with named arguments passed to
  [`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md)

- check.names:

  Column names are based on `cc$group`, which might include spaces.
  Those names are changed to syntactically correct names by
  `check.names = TRUE`. Syntactically invalid, but grammatically correct
  names might be preferred for presentation of the data as achieved by
  `check.names = FALSE`. Alternatively, if `categorize` is called
  repeatedly, longer informative names might be created by
  `cc_args = list(tech_names = TRUE)`.

- .data_cols:

  used internally

## Value

Object of the same class as `x` with additional logical columns
indicating membership of groups identified by the `classcodes` object
(the `cc` argument). Numeric indices are also included if requested by
the `index` argument.

## See also

Other verbs:
[`classify()`](https://docs.ropensci.org/coder/reference/classify.md),
[`codify()`](https://docs.ropensci.org/coder/reference/codify.md),
[`index_fun`](https://docs.ropensci.org/coder/reference/index_fun.md)

## Examples

``` r
# For this example, 1 core would suffice:
old_threads <- data.table::getDTthreads()
data.table::setDTthreads(1)

# For some patient data (ex_people) and related hospital visit code data
# with ICD 10-codes (ex_icd10), add the Elixhauser comorbidity
# conditions based on all registered ICD10-codes
categorize(
   x            = ex_people,
   codedata     = ex_icd10,
   cc           = "elixhauser",
   id           = "name",
   code         = "icd10"
)
#> Classification based on: icd10
#> # A tibble: 100 × 40
#>    name   surgery    congestive.heart.fai…¹ cardiac.arrhythmias valvular.disease
#>    <chr>  <date>     <lgl>                  <lgl>               <lgl>           
#>  1 Chen,… 2025-04-21 FALSE                  FALSE               FALSE           
#>  2 Grave… 2025-01-11 FALSE                  FALSE               FALSE           
#>  3 Truji… 2024-12-29 FALSE                  FALSE               FALSE           
#>  4 Simps… 2025-04-02 FALSE                  FALSE               FALSE           
#>  5 Chin,… 2025-03-16 FALSE                  FALSE               FALSE           
#>  6 Le, C… 2024-10-18 FALSE                  FALSE               FALSE           
#>  7 Kang,… 2025-01-20 FALSE                  FALSE               FALSE           
#>  8 Shuem… 2024-10-19 FALSE                  FALSE               FALSE           
#>  9 Bouch… 2025-03-27 FALSE                  FALSE               FALSE           
#> 10 Le, S… 2025-03-01 FALSE                  FALSE               FALSE           
#> # ℹ 90 more rows
#> # ℹ abbreviated name: ¹​congestive.heart.failure
#> # ℹ 35 more variables: pulmonary.circulation.disorder <lgl>,
#> #   peripheral.vascular.disorder <lgl>, hypertension.uncomplicated <lgl>,
#> #   hypertension.complicated <lgl>, paralysis <lgl>,
#> #   other.neurological.disorders <lgl>, chronic.pulmonary.disease <lgl>,
#> #   diabetes.uncomplicated <lgl>, diabetes.complicated <lgl>, …


# Add Charlson categories and two versions of a calculated index
# ("quan_original" and "quan_updated").
categorize(
   x            = ex_people,
   codedata     = ex_icd10,
   cc           = "charlson",
   id           = "name",
   code         = "icd10",
   index        = c("quan_original", "quan_updated")
)
#> Classification based on: icd10
#> # A tibble: 100 × 21
#>    name              surgery    myocardial.infarction congestive.heart.failure
#>    <chr>             <date>     <lgl>                 <lgl>                   
#>  1 Chen, Trevor      2025-04-21 FALSE                 FALSE                   
#>  2 Graves, Acineth   2025-01-11 FALSE                 FALSE                   
#>  3 Trujillo, Yanelly 2024-12-29 FALSE                 FALSE                   
#>  4 Simpson, Kenneth  2025-04-02 FALSE                 FALSE                   
#>  5 Chin, Nelson      2025-03-16 FALSE                 FALSE                   
#>  6 Le, Christina     2024-10-18 FALSE                 FALSE                   
#>  7 Kang, Xuan        2025-01-20 FALSE                 FALSE                   
#>  8 Shuemaker, Lauren 2024-10-19 FALSE                 FALSE                   
#>  9 Boucher, Teresa   2025-03-27 FALSE                 FALSE                   
#> 10 Le, Soraiya       2025-03-01 FALSE                 FALSE                   
#> # ℹ 90 more rows
#> # ℹ 17 more variables: peripheral.vascular.disease <lgl>,
#> #   cerebrovascular.disease <lgl>, dementia <lgl>,
#> #   chronic.pulmonary.disease <lgl>, rheumatic.disease <lgl>,
#> #   peptic.ulcer.disease <lgl>, mild.liver.disease <lgl>,
#> #   diabetes.without.complication <lgl>, hemiplegia.or.paraplegia <lgl>,
#> #   renal.disease <lgl>, diabetes.complication <lgl>, malignancy <lgl>, …


# Only include recent hospital visits within 30 days before surgery,
categorize(
   x            = ex_people,
   codedata     = ex_icd10,
   cc           = "charlson",
   id           = "name",
   code         = "icd10",
   index        = c("quan_original", "quan_updated"),
   codify_args  = list(
      date      = "surgery",
      days      = c(-30, -1),
      code_date = "admission"
   )
)
#> Classification based on: icd10
#> # A tibble: 100 × 21
#>    name              surgery    myocardial.infarction congestive.heart.failure
#>    <chr>             <date>     <lgl>                 <lgl>                   
#>  1 Chen, Trevor      2025-04-21 FALSE                 FALSE                   
#>  2 Graves, Acineth   2025-01-11 NA                    NA                      
#>  3 Trujillo, Yanelly 2024-12-29 NA                    NA                      
#>  4 Simpson, Kenneth  2025-04-02 FALSE                 FALSE                   
#>  5 Chin, Nelson      2025-03-16 FALSE                 FALSE                   
#>  6 Le, Christina     2024-10-18 FALSE                 FALSE                   
#>  7 Kang, Xuan        2025-01-20 FALSE                 FALSE                   
#>  8 Shuemaker, Lauren 2024-10-19 FALSE                 FALSE                   
#>  9 Boucher, Teresa   2025-03-27 NA                    NA                      
#> 10 Le, Soraiya       2025-03-01 FALSE                 FALSE                   
#> # ℹ 90 more rows
#> # ℹ 17 more variables: peripheral.vascular.disease <lgl>,
#> #   cerebrovascular.disease <lgl>, dementia <lgl>,
#> #   chronic.pulmonary.disease <lgl>, rheumatic.disease <lgl>,
#> #   peptic.ulcer.disease <lgl>, mild.liver.disease <lgl>,
#> #   diabetes.without.complication <lgl>, hemiplegia.or.paraplegia <lgl>,
#> #   renal.disease <lgl>, diabetes.complication <lgl>, malignancy <lgl>, …



# Multiple versions -------------------------------------------------------

# We can compare categorization by according to Quan et al. (2005); "icd10",
# and Armitage et al. (2010); "icd10_rcs" (see `?charlson`)
# Note the use of `tech_names = TRUE` to distinguish the column names from the
# two versions.

# We first specify some common settings ...
ind <- c("quan_original", "quan_updated")
cd  <- list(date = "surgery", days = c(-30, -1), code_date = "admission")

# ... we then categorize once with "icd10" as the default regular expression ...
categorize(
   x            = ex_people,
   codedata     = ex_icd10,
   cc           = "charlson",
   id           = "name",
   code         = "icd10",
   index        = ind,
   codify_args  = cd,
   cc_args      = list(tech_names = TRUE)
) %>%

# .. and once more with `regex = "icd10_rcs"`
categorize(
   codedata     = ex_icd10,
   cc           = "charlson",
   id           = "name",
   code         = "icd10",
   index        = ind,
   codify_args  = cd,
   cc_args      = list(regex = "icd10_rcs", tech_names = TRUE)
)
#> Classification based on: icd10
#> # A tibble: 100 × 37
#>    name              surgery    charlson_icd10_myocardi…¹ charlson_icd10_conge…²
#>    <chr>             <date>     <lgl>                     <lgl>                 
#>  1 Chen, Trevor      2025-04-21 FALSE                     FALSE                 
#>  2 Graves, Acineth   2025-01-11 NA                        NA                    
#>  3 Trujillo, Yanelly 2024-12-29 NA                        NA                    
#>  4 Simpson, Kenneth  2025-04-02 FALSE                     FALSE                 
#>  5 Chin, Nelson      2025-03-16 FALSE                     FALSE                 
#>  6 Le, Christina     2024-10-18 FALSE                     FALSE                 
#>  7 Kang, Xuan        2025-01-20 FALSE                     FALSE                 
#>  8 Shuemaker, Lauren 2024-10-19 FALSE                     FALSE                 
#>  9 Boucher, Teresa   2025-03-27 NA                        NA                    
#> 10 Le, Soraiya       2025-03-01 FALSE                     FALSE                 
#> # ℹ 90 more rows
#> # ℹ abbreviated names: ¹​charlson_icd10_myocardial_infarction,
#> #   ²​charlson_icd10_congestive_heart_failure
#> # ℹ 33 more variables: charlson_icd10_peripheral_vascular_disease <lgl>,
#> #   charlson_icd10_cerebrovascular_disease <lgl>,
#> #   charlson_icd10_dementia <lgl>,
#> #   charlson_icd10_chronic_pulmonary_disease <lgl>, …



# column names ------------------------------------------------------------

# Default column names are based on row names from corresponding classcodes
# object but are modified to be syntactically correct.
default <-
   categorize(ex_people, codedata = ex_icd10, cc = "elixhauser",
              id = "name", code = "icd10")
#> Classification based on: icd10

# Set `check.names = FALSE` to retain original names:
original <-
  categorize(
    ex_people, codedata = ex_icd10, cc = "elixhauser",
    id = "name", code = "icd10",
    check.names = FALSE
   )
#> Classification based on: icd10

# Or use `tech_names = TRUE` for informative but long names (use case above)
tech <-
  categorize(ex_people, codedata = ex_icd10, cc = "elixhauser",
    id = "name", code = "icd10",
    cc_args = list(tech_names = TRUE)
  )
#> Classification based on: icd10

# Compare
tibble::tibble(names(default), names(original), names(tech))
#> # A tibble: 40 × 3
#>    `names(default)`               `names(original)`              `names(tech)`  
#>    <chr>                          <chr>                          <chr>          
#>  1 name                           name                           name           
#>  2 surgery                        surgery                        surgery        
#>  3 congestive.heart.failure       congestive heart failure       elixhauser_icd…
#>  4 cardiac.arrhythmias            cardiac arrhythmias            elixhauser_icd…
#>  5 valvular.disease               valvular disease               elixhauser_icd…
#>  6 pulmonary.circulation.disorder pulmonary circulation disorder elixhauser_icd…
#>  7 peripheral.vascular.disorder   peripheral vascular disorder   elixhauser_icd…
#>  8 hypertension.uncomplicated     hypertension uncomplicated     elixhauser_icd…
#>  9 hypertension.complicated       hypertension complicated       elixhauser_icd…
#> 10 paralysis                      paralysis                      elixhauser_icd…
#> # ℹ 30 more rows

# Go back to original number of threads
data.table::setDTthreads(old_threads)
```
