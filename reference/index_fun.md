# Calculate index based on classification scheme

This is the third step of `codify() %>% classify() %>% index()`. The
function takes classified case data and calculates (weighted) index sums
as specified by weights from a `classcodes` object.

## Usage

``` r
index(classified, ...)

# S3 method for class 'data.frame'
index(classified, ...)

# S3 method for class 'matrix'
index(classified, index = NULL, cc = NULL, ...)
```

## Arguments

- classified:

  output from
  [`classify()`](https://docs.ropensci.org/coder/reference/classify.md)

- ...:

  used internally

- index:

  name of column with 'weights' from corresponding
  [`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md)
  object. Can be `NULL` if the index is just a unweighted count of all
  identified groups.

- cc:

  [`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md)
  object. Can be `NULL` if a `classcodes` object is already available as
  an attribute of `classified` (which is often the case) and/or if
  `index = NULL`.

## Value

Named numeric index vector with names corresponding to
`rownames(classified)`

## Details

Index weights for subordinate hierarchical classes (as identified by
`attr(cc, "hierarchy")`) are excluded in presence of superior classes if
index specified with argument `index`.

## See also

Other verbs:
[`categorize()`](https://docs.ropensci.org/coder/reference/categorize.md),
[`classify()`](https://docs.ropensci.org/coder/reference/classify.md),
[`codify()`](https://docs.ropensci.org/coder/reference/codify.md)

## Examples

``` r

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

# Classify those patients by the Charlson comorbidity indices
cl <- classify(x, "charlson")
#> Warning: 'classify()' does not preserve row order ('categorize()' does!)
#> Classification based on: icd10

# Calculate (weighted) index values
head(index(cl))                  # Un-weighted sum/no of conditions for each patient
#> index calculated as number of relevant categories
#>       Connelly, Monta  Garcia Hyett, Brenda Martinez Botello, Luz 
#>                     0                     0                     0 
#>     Marton, Gabriella        Mitchell, John          Nelson, Cory 
#>                     0                     0                     0 
head(index(cl, "quan_original")) # Weighted index (Quan et al. 2005; see `?charlson`)
#>       Connelly, Monta  Garcia Hyett, Brenda Martinez Botello, Luz 
#>                     0                     0                     0 
#>     Marton, Gabriella        Mitchell, John          Nelson, Cory 
#>                     0                     0                     0 
head(index(cl, "quan_updated"))  # Weighted index (Quan et al. 2011; see `?charlson`)
#>       Connelly, Monta  Garcia Hyett, Brenda Martinez Botello, Luz 
#>                     0                     0                     0 
#>     Marton, Gabriella        Mitchell, John          Nelson, Cory 
#>                     0                     0                     0 

# Tabulate index for all patients.
# As expected, most patients are healthy and have index = 0/NA,
# where NA indicates no recorded hospital visits
# found in `ex_icd10` during codification.
# In practice, those patients might be assumed to have 0 comorbidity as well.
table(index(cl, "quan_original"), useNA = "always")
#> 
#>    0    1    2    3    6 <NA> 
#>   74    5    4    1    1   15 

# If `cl` is a matrix without additional attributes (as imposed by `codify()`)
# an explicit classcodes object must be specified by the `cc` argument
cl2 <- as.matrix(cl)
head(index(cl2, cc = "charlson"))
#> index calculated as number of relevant categories
#>       Connelly, Monta  Garcia Hyett, Brenda Martinez Botello, Luz 
#>                     0                     0                     0 
#>     Marton, Gabriella        Mitchell, John          Nelson, Cory 
#>                     0                     0                     0 
```
