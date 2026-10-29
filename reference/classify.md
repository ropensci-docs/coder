# Classify codified data

This is the second step of `codify() %>% classify() %>% index()`. Hence,
the function takes a codified data set and classify each case based on
relevant codes as identified by the classification scheme provided by a
`classcodes` object.

## Usage

``` r
classify(codified, cc, ..., cc_args = list())

# Default S3 method
classify(codified, cc, ..., cc_args = list())

# S3 method for class 'codified'
classify(codified, ...)

# S3 method for class 'data.frame'
classify(codified, ...)

# S3 method for class 'data.table'
classify(codified, cc, ..., id, code, cc_args = list())
```

## Arguments

- codified:

  output from
  [`codify()`](https://docs.ropensci.org/coder/reference/codify.md)

- cc:

  [`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md)
  object (or name of a default object from
  [`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md)).

- ...:

  arguments passed between methods

- cc_args:

  List with named arguments passed to
  [`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md)

- code, id:

  name of code/id columns (in `codified`).

## Value

Object of class "classified". Inheriting from a Boolean matrix with one
row for each element/row of `codified` and columns for each class with
corresponding class names (according to the
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md)
object). Note, however, that
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md)
preview this output as a tibble.

## See also

[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`as.data.table.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md)
and
[`as.matrix.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md)

Other verbs:
[`categorize()`](https://docs.ropensci.org/coder/reference/categorize.md),
[`codify()`](https://docs.ropensci.org/coder/reference/codify.md),
[`index_fun`](https://docs.ropensci.org/coder/reference/index_fun.md)

## Examples

``` r


# classify.default() ------------------------------------------------------

# Classify individual ICD10-codes by Elixhauser
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



# classify.codified() -----------------------------------------------------

# Prepare some codified data with ICD-10 codes during 1 year (365 days)
# before surgery
x <-
  codify(
    ex_people,
    ex_icd10,
    id        = "name",
    code      = "icd10",
    date      = "surgery",
    days      = c(-365, 0),
    code_date = "admission"
  )

# Classify those patients by the Charlson and Elixhasuer comorbidity indices
classify(x, "charlson")        # classcodes object by name ...
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)
#> Classification based on: icd10
#> 
#> The printed data is of class: classified, matrix.
#> It has 100 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 17
#>    `myocardial infarction` `congestive heart failure` peripheral vascular dise…¹
#>    <lgl>                   <lgl>                      <lgl>                     
#>  1 FALSE                   FALSE                      FALSE                     
#>  2 FALSE                   FALSE                      FALSE                     
#>  3 FALSE                   FALSE                      FALSE                     
#>  4 FALSE                   FALSE                      FALSE                     
#>  5 FALSE                   FALSE                      FALSE                     
#>  6 FALSE                   FALSE                      FALSE                     
#>  7 FALSE                   FALSE                      FALSE                     
#>  8 FALSE                   FALSE                      FALSE                     
#>  9 FALSE                   FALSE                      FALSE                     
#> 10 FALSE                   FALSE                      FALSE                     
#> # ℹ abbreviated name: ¹​`peripheral vascular disease`
#> # ℹ 14 more variables: `cerebrovascular disease` <lgl>, dementia <lgl>,
#> #   `chronic pulmonary disease` <lgl>, `rheumatic disease` <lgl>,
#> #   `peptic ulcer disease` <lgl>, `mild liver disease` <lgl>,
#> #   `diabetes without complication` <lgl>, `hemiplegia or paraplegia` <lgl>,
#> #   `renal disease` <lgl>, `diabetes complication` <lgl>, malignancy <lgl>,
#> #   `moderate or severe liver disease` <lgl>, `metastatic solid tumor` <lgl>, …
classify(x, coder::elixhauser) # ... or by the object itself
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)
#> Classification based on: icd10
#> 
#> The printed data is of class: classified, matrix.
#> It has 100 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 31
#>    `congestive heart failure` `cardiac arrhythmias` `valvular disease`
#>    <lgl>                      <lgl>                 <lgl>             
#>  1 FALSE                      FALSE                 FALSE             
#>  2 FALSE                      FALSE                 FALSE             
#>  3 FALSE                      FALSE                 FALSE             
#>  4 FALSE                      FALSE                 FALSE             
#>  5 FALSE                      FALSE                 FALSE             
#>  6 FALSE                      FALSE                 FALSE             
#>  7 FALSE                      FALSE                 FALSE             
#>  8 FALSE                      FALSE                 FALSE             
#>  9 FALSE                      FALSE                 FALSE             
#> 10 NA                         NA                    NA                
#> # ℹ 28 more variables: `pulmonary circulation disorder` <lgl>,
#> #   `peripheral vascular disorder` <lgl>, `hypertension uncomplicated` <lgl>,
#> #   `hypertension complicated` <lgl>, paralysis <lgl>,
#> #   `other neurological disorders` <lgl>, `chronic pulmonary disease` <lgl>,
#> #   `diabetes uncomplicated` <lgl>, `diabetes complicated` <lgl>,
#> #   hypothyroidism <lgl>, `renal failure` <lgl>, `liver disease` <lgl>,
#> #   `peptic ulcer disease` <lgl>, `AIDS/HIV` <lgl>, lymphoma <lgl>, …


# -- start/stop --
# Assume that a prefix "ICD-10 = " is used for all codes and that some
# additional numbers are added to the end
x$icd10 <- paste0("ICD-10 = ", x$icd10)

# Set start = FALSE to identify codes which are not necessarily found in the
# beginning of the string
classify(x, "charlson", cc_args = list(start = FALSE))
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)
#> Classification based on: icd10
#> 
#> The printed data is of class: classified, matrix.
#> It has 100 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 17
#>    `myocardial infarction` `congestive heart failure` peripheral vascular dise…¹
#>    <lgl>                   <lgl>                      <lgl>                     
#>  1 FALSE                   FALSE                      FALSE                     
#>  2 FALSE                   FALSE                      FALSE                     
#>  3 FALSE                   FALSE                      FALSE                     
#>  4 FALSE                   FALSE                      FALSE                     
#>  5 FALSE                   FALSE                      FALSE                     
#>  6 FALSE                   FALSE                      FALSE                     
#>  7 FALSE                   FALSE                      FALSE                     
#>  8 FALSE                   FALSE                      FALSE                     
#>  9 FALSE                   FALSE                      FALSE                     
#> 10 FALSE                   FALSE                      FALSE                     
#> # ℹ abbreviated name: ¹​`peripheral vascular disease`
#> # ℹ 14 more variables: `cerebrovascular disease` <lgl>, dementia <lgl>,
#> #   `chronic pulmonary disease` <lgl>, `rheumatic disease` <lgl>,
#> #   `peptic ulcer disease` <lgl>, `mild liver disease` <lgl>,
#> #   `diabetes without complication` <lgl>, `hemiplegia or paraplegia` <lgl>,
#> #   `renal disease` <lgl>, `diabetes complication` <lgl>, malignancy <lgl>,
#> #   `moderate or severe liver disease` <lgl>, `metastatic solid tumor` <lgl>, …


# -- regex --
# Use a different version of Charlson (as formulated by regular expressions
# according to the Royal College of Surgeons (RCS) by passing arguments to
# `set_classcodes()` using the `cc_args` argument
y <-
  classify(
    x,
    "charlson",
    cc_args = list(regex = "icd10_rcs")
  )
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)


# -- tech_names --
# Assume that we want to compare the results using the default ICD-10
# formulations (from Quan et al. 2005) and the RCS version and that the result
# should be put into the same data frame. We can use `tech_names = TRUE`
# to distinguish variables with otherwise similar names
cc <- list(tech_names = TRUE) # Prepare sommon settings
compare <-
  merge(
  classify(x, "charlson", cc_args = cc),
  classify(x, "charlson", cc_args = c(cc, regex = "icd10_rcs"))
)
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)
#> Classification based on: icd10
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)
names(compare) # long but informative and distinguishable column names
#>  [1] "name"                                               
#>  [2] "charlson_icd10_myocardial_infarction"               
#>  [3] "charlson_icd10_congestive_heart_failure"            
#>  [4] "charlson_icd10_peripheral_vascular_disease"         
#>  [5] "charlson_icd10_cerebrovascular_disease"             
#>  [6] "charlson_icd10_dementia"                            
#>  [7] "charlson_icd10_chronic_pulmonary_disease"           
#>  [8] "charlson_icd10_rheumatic_disease"                   
#>  [9] "charlson_icd10_peptic_ulcer_disease"                
#> [10] "charlson_icd10_mild_liver_disease"                  
#> [11] "charlson_icd10_diabetes_without_complication"       
#> [12] "charlson_icd10_hemiplegia_or_paraplegia"            
#> [13] "charlson_icd10_renal_disease"                       
#> [14] "charlson_icd10_diabetes_complication"               
#> [15] "charlson_icd10_malignancy"                          
#> [16] "charlson_icd10_moderate_or_severe_liver_disease"    
#> [17] "charlson_icd10_metastatic_solid_tumor"              
#> [18] "charlson_icd10_aids_hiv"                            
#> [19] "charlson_icd10_rcs_myocardial_infarction"           
#> [20] "charlson_icd10_rcs_congestive_heart_failure"        
#> [21] "charlson_icd10_rcs_peripheral_vascular_disease"     
#> [22] "charlson_icd10_rcs_cerebrovascular_disease"         
#> [23] "charlson_icd10_rcs_dementia"                        
#> [24] "charlson_icd10_rcs_chronic_pulmonary_disease"       
#> [25] "charlson_icd10_rcs_rheumatic_disease"               
#> [26] "charlson_icd10_rcs_hemiplegia_or_paraplegia"        
#> [27] "charlson_icd10_rcs_renal_disease"                   
#> [28] "charlson_icd10_rcs_diabetes_complication"           
#> [29] "charlson_icd10_rcs_malignancy"                      
#> [30] "charlson_icd10_rcs_moderate_or_severe_liver_disease"
#> [31] "charlson_icd10_rcs_metastatic_solid_tumor"          
#> [32] "charlson_icd10_rcs_aids_hiv"                        



# classify.data.frame() / classify.data.table() ------------------------

# Assume that `x` is a data.frame/data.table without additional attributes
# from `codify()` ...
xdf <- as.data.frame(x)
xdt <- data.table::as.data.table(x)

# ... then the `id` and `code` columns must be specified explicitly
classify(xdf, "charlson", id = "name", code = "icd10")
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)
#> Classification based on: icd10
#> 
#> The printed data is of class: classified, matrix.
#> It has 100 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 17
#>    `myocardial infarction` `congestive heart failure` peripheral vascular dise…¹
#>    <lgl>                   <lgl>                      <lgl>                     
#>  1 FALSE                   FALSE                      FALSE                     
#>  2 FALSE                   FALSE                      FALSE                     
#>  3 FALSE                   FALSE                      FALSE                     
#>  4 FALSE                   FALSE                      FALSE                     
#>  5 FALSE                   FALSE                      FALSE                     
#>  6 FALSE                   FALSE                      FALSE                     
#>  7 FALSE                   FALSE                      FALSE                     
#>  8 FALSE                   FALSE                      FALSE                     
#>  9 FALSE                   FALSE                      FALSE                     
#> 10 FALSE                   FALSE                      FALSE                     
#> # ℹ abbreviated name: ¹​`peripheral vascular disease`
#> # ℹ 14 more variables: `cerebrovascular disease` <lgl>, dementia <lgl>,
#> #   `chronic pulmonary disease` <lgl>, `rheumatic disease` <lgl>,
#> #   `peptic ulcer disease` <lgl>, `mild liver disease` <lgl>,
#> #   `diabetes without complication` <lgl>, `hemiplegia or paraplegia` <lgl>,
#> #   `renal disease` <lgl>, `diabetes complication` <lgl>, malignancy <lgl>,
#> #   `moderate or severe liver disease` <lgl>, `metastatic solid tumor` <lgl>, …
classify(xdt, "charlson", id = "name", code = "icd10")
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)
#> Classification based on: icd10
#> 
#> The printed data is of class: classified, matrix.
#> It has 100 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 17
#>    `myocardial infarction` `congestive heart failure` peripheral vascular dise…¹
#>    <lgl>                   <lgl>                      <lgl>                     
#>  1 FALSE                   FALSE                      FALSE                     
#>  2 FALSE                   FALSE                      FALSE                     
#>  3 FALSE                   FALSE                      FALSE                     
#>  4 FALSE                   FALSE                      FALSE                     
#>  5 FALSE                   FALSE                      FALSE                     
#>  6 FALSE                   FALSE                      FALSE                     
#>  7 FALSE                   FALSE                      FALSE                     
#>  8 FALSE                   FALSE                      FALSE                     
#>  9 FALSE                   FALSE                      FALSE                     
#> 10 FALSE                   FALSE                      FALSE                     
#> # ℹ abbreviated name: ¹​`peripheral vascular disease`
#> # ℹ 14 more variables: `cerebrovascular disease` <lgl>, dementia <lgl>,
#> #   `chronic pulmonary disease` <lgl>, `rheumatic disease` <lgl>,
#> #   `peptic ulcer disease` <lgl>, `mild liver disease` <lgl>,
#> #   `diabetes without complication` <lgl>, `hemiplegia or paraplegia` <lgl>,
#> #   `renal disease` <lgl>, `diabetes complication` <lgl>, malignancy <lgl>,
#> #   `moderate or severe liver disease` <lgl>, `metastatic solid tumor` <lgl>, …
```
