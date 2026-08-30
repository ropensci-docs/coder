# Set classcodes object

Prepare a `classcodes`object by specifying the regular expressions to
use for classification.

## Usage

``` r
set_classcodes(
  cc,
  classified = NULL,
  regex = NULL,
  start = TRUE,
  stop = FALSE,
  tech_names = NULL
)
```

## Arguments

- cc:

  [`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md)
  object (or name of a default object from
  [`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md)).

- classified:

  object that classcodes could be inherited from

- regex:

  name of column with regular expressions to use for classification.
  `NULL` (default) uses `attr(obj, "regexpr")[1]`.

- start, stop:

  should codes start/end with the specified regular expressions? If
  `TRUE`, column "regex" is prefixed/suffixed by `^/$`.

- tech_names:

  should technical column names be used? If `FALSE`, colnames are taken
  directly from group names of `cc`, if `TRUE`, these are changed to
  more technical names avoiding special characters and are prefixed by
  the name of the classification scheme. `NULL` (by default) preserves
  previous names if `cc` is inherited from `classified` (fall backs to
  `FALSE` if not already set).

## Value

[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md)
object.

## See also

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

## Examples

``` r
# Prepare a classcodes object for the Charlson comorbidity classification
# based on the default regular expressions
set_classcodes(charlson)   # by object
#> Classification based on: icd10
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd10 
#> Indices:
#>    charlson, deyo_ramano, dhoore, ghali, quan_original, quan_updated   
#> 
#> # A tibble: 17 × 9
#>    group       description icd10 charlson deyo_ramano dhoore ghali quan_original
#>    <chr>       <chr>       <chr>    <dbl>       <dbl>  <dbl> <dbl>         <dbl>
#>  1 myocardial… Acute myoc… ^(I2…        1           1      1     1             1
#>  2 congestive… Heart fail… ^(I(…        1           1      1     4             1
#>  3 peripheral… Peripheral… ^(I7…        1           1      1     2             1
#>  4 cerebrovas… Cerebrovas… ^(G4…        1           1      1     1             1
#>  5 dementia    Senile and… ^(F0…        1           1      1     0             1
#>  6 chronic pu… Chronic ob… ^((I…        1           1      1     0             1
#>  7 rheumatic … Systemic l… ^(M(…        1           1      1     0             1
#>  8 peptic ulc… Gastric, d… ^(K2…        1           1      1     0             1
#>  9 mild liver… Alcoholic … ^(B1…        1           1      1     0             1
#> 10 diabetes w… Diabetes w… ^(E1…        1           1      1     0             1
#> 11 hemiplegia… Paraplegia… ^(G(…        2           1      1     0             2
#> 12 renal dise… Chronic gl… ^(I1…        2           1      1     3             2
#> 13 diabetes c… Diabetes w… ^(E1…        2           1      1     0             2
#> 14 malignancy  Malignant … ^(C(…        2           1      1     0             2
#> 15 moderate o… Hepatic co… ^(I(…        3           1      1     0             3
#> 16 metastatic… Secondary … ^(C(…        6           1      1     0             6
#> 17 AIDS/HIV    HIV infect… ^(B2…        6           1      1     0             6
#> # ℹ 1 more variable: quan_updated <dbl>
set_classcodes("charlson") # by name
#> Classification based on: icd10
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd10 
#> Indices:
#>    charlson, deyo_ramano, dhoore, ghali, quan_original, quan_updated   
#> 
#> # A tibble: 17 × 9
#>    group       description icd10 charlson deyo_ramano dhoore ghali quan_original
#>    <chr>       <chr>       <chr>    <dbl>       <dbl>  <dbl> <dbl>         <dbl>
#>  1 myocardial… Acute myoc… ^(I2…        1           1      1     1             1
#>  2 congestive… Heart fail… ^(I(…        1           1      1     4             1
#>  3 peripheral… Peripheral… ^(I7…        1           1      1     2             1
#>  4 cerebrovas… Cerebrovas… ^(G4…        1           1      1     1             1
#>  5 dementia    Senile and… ^(F0…        1           1      1     0             1
#>  6 chronic pu… Chronic ob… ^((I…        1           1      1     0             1
#>  7 rheumatic … Systemic l… ^(M(…        1           1      1     0             1
#>  8 peptic ulc… Gastric, d… ^(K2…        1           1      1     0             1
#>  9 mild liver… Alcoholic … ^(B1…        1           1      1     0             1
#> 10 diabetes w… Diabetes w… ^(E1…        1           1      1     0             1
#> 11 hemiplegia… Paraplegia… ^(G(…        2           1      1     0             2
#> 12 renal dise… Chronic gl… ^(I1…        2           1      1     3             2
#> 13 diabetes c… Diabetes w… ^(E1…        2           1      1     0             2
#> 14 malignancy  Malignant … ^(C(…        2           1      1     0             2
#> 15 moderate o… Hepatic co… ^(I(…        3           1      1     0             3
#> 16 metastatic… Secondary … ^(C(…        6           1      1     0             6
#> 17 AIDS/HIV    HIV infect… ^(B2…        6           1      1     0             6
#> # ℹ 1 more variable: quan_updated <dbl>

# Same as above but based on regular expressions for ICD-8 (see `?charlson`)
set_classcodes(charlson, regex = "icd8_brusselaers")
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd8_brusselaers 
#> Indices:
#>    charlson, deyo_ramano, dhoore, ghali, quan_original, quan_updated   
#> 
#> # A tibble: 13 × 9
#>    group          description icd8_brusselaers charlson deyo_ramano dhoore ghali
#>    <chr>          <chr>       <chr>               <dbl>       <dbl>  <dbl> <dbl>
#>  1 myocardial in… Acute myoc… ^(41[0-2])              1           1      1     1
#>  2 congestive he… Heart fail… ^(4270|428)             1           1      1     4
#>  3 peripheral va… Peripheral… ^(44[0-5])              1           1      1     2
#>  4 cerebrovascul… Cerebrovas… ^(43[0-8])              1           1      1     1
#>  5 dementia       Senile and… ^(290[01])              1           1      1     0
#>  6 chronic pulmo… Chronic ob… ^(49[0-3]|51[5-…        1           1      1     0
#>  7 rheumatic dis… Systemic l… ^(7(1[0-2]|34))         1           1      1     0
#>  8 hemiplegia or… Paraplegia… ^(344)                  2           1      1     0
#>  9 renal disease  Chronic gl… ^(40[34]|58[0-3…        2           1      1     3
#> 10 diabetes comp… Diabetes w… ^(250)                  2           1      1     0
#> 11 malignancy     Malignant … ^(1([4-68][0-9]…        2           1      1     0
#> 12 moderate or s… Hepatic co… ^(070|4560|51[1…        3           1      1     0
#> 13 metastatic so… Secondary … ^(19[6-9])              6           1      1     0
#> # ℹ 2 more variables: quan_original <dbl>, quan_updated <dbl>

# Only recognize codes if no other characters are found after the relevant codes
# Hence if the code vector stops with the code
set_classcodes(charlson, stop = TRUE)
#> Classification based on: icd10
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd10 
#> Indices:
#>    charlson, deyo_ramano, dhoore, ghali, quan_original, quan_updated   
#> 
#> # A tibble: 17 × 9
#>    group       description icd10 charlson deyo_ramano dhoore ghali quan_original
#>    <chr>       <chr>       <chr>    <dbl>       <dbl>  <dbl> <dbl>         <dbl>
#>  1 myocardial… Acute myoc… ^(I2…        1           1      1     1             1
#>  2 congestive… Heart fail… ^(I(…        1           1      1     4             1
#>  3 peripheral… Peripheral… ^(I7…        1           1      1     2             1
#>  4 cerebrovas… Cerebrovas… ^(G4…        1           1      1     1             1
#>  5 dementia    Senile and… ^(F0…        1           1      1     0             1
#>  6 chronic pu… Chronic ob… ^((I…        1           1      1     0             1
#>  7 rheumatic … Systemic l… ^(M(…        1           1      1     0             1
#>  8 peptic ulc… Gastric, d… ^(K2…        1           1      1     0             1
#>  9 mild liver… Alcoholic … ^(B1…        1           1      1     0             1
#> 10 diabetes w… Diabetes w… ^(E1…        1           1      1     0             1
#> 11 hemiplegia… Paraplegia… ^(G(…        2           1      1     0             2
#> 12 renal dise… Chronic gl… ^(I1…        2           1      1     3             2
#> 13 diabetes c… Diabetes w… ^(E1…        2           1      1     0             2
#> 14 malignancy  Malignant … ^(C(…        2           1      1     0             2
#> 15 moderate o… Hepatic co… ^(I(…        3           1      1     0             3
#> 16 metastatic… Secondary … ^(C(…        6           1      1     0             6
#> 17 AIDS/HIV    HIV infect… ^(B2…        6           1      1     0             6
#> # ℹ 1 more variable: quan_updated <dbl>

# Accept code vectors with strings which do not necessarily start with the code.
# This is useful if the code might appear in the middle of a longer character
# string or if a common prefix is used for all codes.
set_classcodes(charlson, start = FALSE)
#> Classification based on: icd10
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd10 
#> Indices:
#>    charlson, deyo_ramano, dhoore, ghali, quan_original, quan_updated   
#> 
#> # A tibble: 17 × 9
#>    group       description icd10 charlson deyo_ramano dhoore ghali quan_original
#>    <chr>       <chr>       <chr>    <dbl>       <dbl>  <dbl> <dbl>         <dbl>
#>  1 myocardial… Acute myoc… I2([…        1           1      1     1             1
#>  2 congestive… Heart fail… I(09…        1           1      1     4             1
#>  3 peripheral… Peripheral… I7([…        1           1      1     2             1
#>  4 cerebrovas… Cerebrovas… G4[5…        1           1      1     1             1
#>  5 dementia    Senile and… F0([…        1           1      1     0             1
#>  6 chronic pu… Chronic ob… (I27…        1           1      1     0             1
#>  7 rheumatic … Systemic l… M(0[…        1           1      1     0             1
#>  8 peptic ulc… Gastric, d… K2[5…        1           1      1     0             1
#>  9 mild liver… Alcoholic … B18|…        1           1      1     0             1
#> 10 diabetes w… Diabetes w… E1[0…        1           1      1     0             1
#> 11 hemiplegia… Paraplegia… G(04…        2           1      1     0             2
#> 12 renal dise… Chronic gl… I1(2…        2           1      1     3             2
#> 13 diabetes c… Diabetes w… E1[0…        2           1      1     0             2
#> 14 malignancy  Malignant … C([0…        2           1      1     0             2
#> 15 moderate o… Hepatic co… I(8(…        3           1      1     0             3
#> 16 metastatic… Secondary … C(7[…        6           1      1     0             6
#> 17 AIDS/HIV    HIV infect… B2[0…        6           1      1     0             6
#> # ℹ 1 more variable: quan_updated <dbl>

# Use technical names to clearly describe the origin of each group.
# Note that the `cc` argument must be specified by a character string
# since this name is used as part of the column names
x <- set_classcodes("charlson", tech_names = TRUE)
#> Classification based on: icd10
x$group
#>  [1] "charlson_icd10_myocardial_infarction"           
#>  [2] "charlson_icd10_congestive_heart_failure"        
#>  [3] "charlson_icd10_peripheral_vascular_disease"     
#>  [4] "charlson_icd10_cerebrovascular_disease"         
#>  [5] "charlson_icd10_dementia"                        
#>  [6] "charlson_icd10_chronic_pulmonary_disease"       
#>  [7] "charlson_icd10_rheumatic_disease"               
#>  [8] "charlson_icd10_peptic_ulcer_disease"            
#>  [9] "charlson_icd10_mild_liver_disease"              
#> [10] "charlson_icd10_diabetes_without_complication"   
#> [11] "charlson_icd10_hemiplegia_or_paraplegia"        
#> [12] "charlson_icd10_renal_disease"                   
#> [13] "charlson_icd10_diabetes_complication"           
#> [14] "charlson_icd10_malignancy"                      
#> [15] "charlson_icd10_moderate_or_severe_liver_disease"
#> [16] "charlson_icd10_metastatic_solid_tumor"          
#> [17] "charlson_icd10_aids_hiv"                        
```
