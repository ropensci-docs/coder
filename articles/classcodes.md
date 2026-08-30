# Classcodes

``` r

library(coder)
```

## Motivating example

Let’s consider some example data (`ex_peopple` and `ex_icd10`) from
[`vignette("ex_data")`](https://docs.ropensci.org/coder/articles/ex_data.md).

Let’s categorize those patients by their Charlson comorbidity:

``` r

categorize(ex_people, codedata = ex_icd10, cc = charlson, id = "name", code = "icd10")
#> Classification based on: icd10
#> # A tibble: 100 × 25
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
#> # ℹ 21 more variables: peripheral.vascular.disease <lgl>,
#> #   cerebrovascular.disease <lgl>, dementia <lgl>,
#> #   chronic.pulmonary.disease <lgl>, rheumatic.disease <lgl>,
#> #   peptic.ulcer.disease <lgl>, mild.liver.disease <lgl>,
#> #   diabetes.without.complication <lgl>, hemiplegia.or.paraplegia <lgl>,
#> #   renal.disease <lgl>, diabetes.complication <lgl>, malignancy <lgl>, …
```

Here, `charlson` (as supplied by the `cc` argument) is a “classcodes”
object containing a classification scheme. This is the specification of
how to match `ex_icd10$icd10` to each condition recognized by the
Charlson comorbidity classification. It is based on regular expressions
(see [`?regex`](https://rdrr.io/r/base/regex.html)).

## Default classcodes

There are 7 default “classcodes” objects in the package (`classcodes`
column below). Each of them might have several versions of regular
expressions (column `regex`) and weighted indices (column `indices`):

``` r

all_classcodes()
#> # A tibble: 7 × 3
#>   classcodes    regex                                                    indices
#>   <chr>         <chr>                                                    <chr>  
#> 1 charlson      icd10, icd9cm_deyo, icd9cm_enhanced, icd10_rcs, icd10_s… "charl…
#> 2 cps           icd10                                                    "only_…
#> 3 elixhauser    icd10, icd10_short, icd9cm, icd9cm_ahrqweb, icd9cm_enha… "sum_a…
#> 4 hip_ae        icd10, kva, icd10_fracture                               ""     
#> 5 hip_ae_hailer icd10, kva                                               ""     
#> 6 knee_ae       icd10, kva                                               ""     
#> 7 rxriskv       atc_pratt, atc_caughey, atc_garland                      "pratt…
```

## classcodes object

Each of those classcodes objects are documented (see for example
[`?charlson`](https://docs.ropensci.org/coder/reference/charlson.md)).
Those objects are basically tibbles (data frames) with some additional
attributes:

``` r

charlson
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd10, icd9cm_deyo, icd9cm_enhanced, icd10_rcs, icd10_swe, icd8_brusselaers, icd9_brusselaers 
#> Indices:
#>    charlson, deyo_ramano, dhoore, ghali, quan_original, quan_updated   
#> 
#> # A tibble: 17 × 15
#>    group       description icd10 icd9cm_deyo icd9cm_enhanced icd10_rcs icd10_swe
#>    <chr>       <chr>       <chr> <chr>       <chr>           <chr>     <chr>    
#>  1 myocardial… Acute myoc… I2([… 41[02]      41[02]          "I2([1-3… I2([12]|…
#>  2 congestive… Heart fail… I(09… 428         39891|4(0(2([0… "I(1[13]… I((1(1[1…
#>  3 peripheral… Peripheral… I7([… 44(39|1)|7… 0930|4(373|4[0… "(I7([0-… (I7([01]…
#>  4 cerebrovas… Cerebrovas… G4[5… 43[0-8]     36234|43[0-8]   "G4[56]|… G45|I6[0…
#>  5 dementia    Senile and… F0([… 290         29(0|41)|3312   "A810|F0… F0([0-3]…
#>  6 chronic pu… Chronic ob… (I27… 490|50([0-… 4(16[89]|9)|50… "(I2[67]… J(4[1-7]…
#>  7 rheumatic … Systemic l… M(0[… 7(1(0[014]… 4465|7(1(0[0-4… "M(0[569… M(0([568…
#>  8 peptic ulc… Gastric, d… K2[5… 53[1-4]     53[1-4]          NA       K2[5-8]  
#>  9 mild liver… Alcoholic … B18|… 571[24-6]   070([23]{2}|[4…  NA       B1[5-9]|…
#> 10 diabetes w… Diabetes w… E1[0… 250[0-37]   250[0-389]       NA       E1([0-4]…
#> 11 hemiplegia… Paraplegia… G(04… 34(41|2)    3(341|4([23]|4… "G(114|8… G(114|8(…
#> 12 renal dise… Chronic gl… I1(2… 58([2568]|… 40(3([019]1)|4… "I1[23]|… I1(20|31…
#> 13 diabetes c… Diabetes w… E1[0… 250[4-6]    250[4-7]        "E1[0-4]" E1(0[2-5…
#> 14 malignancy  Malignant … C([0… (1([4-68]|… 1([4-68]|7[0-2… "C([01]|… C([0-36]…
#> 15 moderate o… Hepatic co… I(8(… 456[01]|57… 456[0-2]|572[2… "B18|I(8… I((85[09…
#> 16 metastatic… Secondary … C(7[… 19([6-8]|9… 19[6-9]         "C(7[7-9… C(7[789]…
#> 17 AIDS/HIV    HIV infect… B2[0… 04[2-4]     04[2-4]         "B2[0-4]" B2[0-4]|…
#> # ℹ 8 more variables: icd8_brusselaers <chr>, icd9_brusselaers <chr>,
#> #   charlson <dbl>, deyo_ramano <dbl>, dhoore <dbl>, ghali <dbl>,
#> #   quan_original <dbl>, quan_updated <dbl>
```

Columns have pre-specified names and/or content:

- `group`: short descriptive names of all groups to classify by
  (i.e. medical conditions/comorbidities in the Charlson case)
- `description:` (optional) details describing each group
- regular expressions identifying each group (see
  [`vignette("Interpret_regular_expressions")`](https://docs.ropensci.org/coder/articles/Interpret_regular_expressions.md)
  for details and
  [`?charlson`](https://docs.ropensci.org/coder/reference/charlson.md)
  for concrete examples). Multiple versions might be used if combined
  with different code sets (i.e. ICD-9 versus ICD-10) or as suggested by
  different sources/authors. (Column names are arbitrary but identified
  by `attr(., "regexprs")` and specified by argument `regex` in
  [`as.classcodes()`](https://docs.ropensci.org/coder/reference/classcodes.md)).
- numeric vectors used as weights when calculating index sums based on
  all (or a subset of) individual groups. (Column names are arbitrary
  but identified by `attr(., "indices")` and specified by argument
  `indices` in
  [`as.classcodes()`](https://docs.ropensci.org/coder/reference/classcodes.md).)
- `condition`: (optional) conditional classification (not used with
  `charlson` but see example below).

In the example above, we did not specify which version of the regular
expressions to use. We see from the printed output above (or by
`attr(charlson, "regexprs")`), that the first regular expression is
“icd10”. This will be used by default. We have ICD-10 codes recorded in
our code data set (`ex_icd10$icd10`). We might therefore use either
“icd10” or the alternative “icd10_rcs”. Other versions might be relevant
if the medical data is coded by other codes (such as earlier versions of
ICD). We will show below how to alter this setting in practice.

## Hierarchy

Some classcodes objects have an additional class attribute “hierarchy”,
controlling hierarchical groups where only one of possibly several
groups should be used in weighted index sums. The classcodes object for
the Elixhauser comorbidity classification has this property:  

``` r

print(elixhauser, n = 0) # preview 0 rows but present the attributes
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    icd10, icd10_short, icd9cm, icd9cm_ahrqweb, icd9cm_enhanced 
#> Indices:
#>    sum_all, sum_all_ahrq, walraven, sid29, sid30, ahrq_mort, ahrq_readm 
#> Hierarchy:
#>    c("metastatic cancer", "solid tumor"),
#>    c("diabetes uncomplicated", "diabetes complicated")
```

This means that patients who have both metastatic cancer and solid
tumors should be recognized as such if classified. If such patient are
assigned an aggregated index score, however, only the largest score is
used (in this case for a metastatic cancer as superior to a solid
tumor). The same is true for patients diagnosed with both uncomplicated
and complicated diabetes.

Consider a patient Alice with some diagnoses:

``` r

pat <- tibble::tibble(id = "Alice")
diags <- c("C01", "C801", "E1010", "E1021")
decoder::decode(diags, decoder::icd10cm)
#> [1] "Malignant neoplasm of base of tongue"                   
#> [2] "Malignant (primary) neoplasm, unspecified"              
#> [3] "Type 1 diabetes mellitus with ketoacidosis without coma"
#> [4] "Type 1 diabetes mellitus with diabetic nephropathy"
```

According to Elixhauser, poor Alice has both a solid tumor and a
metastatic cancer, as well as diabetes both with and without
complications. The (unweighted) index “sum_all”, however will not equal
4 but 2, since metastatic cancer and diabetes with complications subsume
solid tumors and diabetes without complications.

``` r

icd10 <- tibble::tibble(id = "Alice", icd10 = diags)
x <- categorize(pat, codedata = icd10, cc = elixhauser, 
                id = "id", code = "icd10", index = "sum_all", check.names = FALSE)
#> Classification based on: icd10
t(x)
#>                                [,1]   
#> id                             "Alice"
#> congestive heart failure       "FALSE"
#> cardiac arrhythmias            "FALSE"
#> valvular disease               "FALSE"
#> pulmonary circulation disorder "FALSE"
#> peripheral vascular disorder   "FALSE"
#> hypertension uncomplicated     "FALSE"
#> hypertension complicated       "FALSE"
#> paralysis                      "FALSE"
#> other neurological disorders   "FALSE"
#> chronic pulmonary disease      "FALSE"
#> diabetes uncomplicated         "TRUE" 
#> diabetes complicated           "TRUE" 
#> hypothyroidism                 "FALSE"
#> renal failure                  "FALSE"
#> liver disease                  "FALSE"
#> peptic ulcer disease           "FALSE"
#> AIDS/HIV                       "FALSE"
#> lymphoma                       "FALSE"
#> metastatic cancer              "TRUE" 
#> solid tumor                    "TRUE" 
#> rheumatoid arthritis           "FALSE"
#> coagulopathy                   "FALSE"
#> obesity                        "FALSE"
#> weight loss                    "FALSE"
#> fluid electrolyte disorders    "FALSE"
#> blood loss anemia              "FALSE"
#> deficiency anemia              "FALSE"
#> alcohol abuse                  "FALSE"
#> drug abuse                     "FALSE"
#> psychoses                      "FALSE"
#> depression                     "FALSE"
#> sum_all                        "2"
```

## Conditions

Consider Alice once more. Suppose she got a THA and had some surgical
procedure codes recorded at hospital visits either before, during or
after her index surgery. Those codes are recorded by the Nomesco
classification of surgical procedures (also known as KVA codes in
Swedish). Here, “post_op” indicates whether the code was recorded after
surgery or not. This information is not always accessible by pure date
stamps (if so, the approach illustrated in
[`vignette("coder")`](https://docs.ropensci.org/coder/articles/coder.md)
could be used instead).

``` r


nomesco <- 
  tibble::tibble(
    id      = "Alice",
    kva     = c("AA01", "NFC01"),
    post_op = c(TRUE, FALSE)
  )
```

Thus, the “post_op” column is a Boolean/logical vector with a name
recognized from the “condition” column in `hip_ae`, a classcodes object
used to identify adverse events after THA (the use of
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md)
is further explained below and is used here since `hip_ae` includes
codes for both ICD and NOMESCO/KVA).

``` r

set_classcodes(hip_ae, regex = "kva")
#> 
#> Classcodes object
#>  
#> Regular expressions:
#>    kva 
#> Indices:
#>       
#> 
#> # A tibble: 1 × 3
#>   group kva                                                            condition
#>   <chr> <chr>                                                          <chr>    
#> 1 KVA   ^(NF([CF-HJ-MS-TW]|A(02|1[12]|2[0-2])|Q09|U[013489]9)|QD(A10|… post_op
```

A code from `nomesco$kva` will only be recognized as an adverse events
if 1) the code is matched by the relevant regular expression, and 2) the
extra condition (from `nomesco$post_op`) is `TRUE.`

We need to specify that codes are based on regular expressions matching
NOMESCO codes. We do this by the `regex` argument passed to
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md)
by the `cc_args` argument.

In the data set (`nomesco`), “AA01” was recorded after surgery but does
not indicate a potential adverse event. “NFC01” is a potential adverse
event but was recorded already before surgery. Therefore, no adverse
event will be recognized in this case.

``` r

categorize(pat, codedata = nomesco, cc = hip_ae, id = "id", code = "kva",
           cc_args = list(regex = "kva"))
#> index calculated as number of relevant categories
#> # A tibble: 1 × 3
#>   id    KVA   index
#>   <chr> <lgl> <dbl>
#> 1 Alice FALSE     0
```

## Use classcodes objects

Most functions do not use the classcodes object themselves, but a
modified version passed through
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md).
This function can be called directly but is more often invoked by
arguments passed by the `cc_args` argument used in other functions (as
in the example above).

### Explicit use of `set_classcodes()`

We might use
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md)
to prepare a classification scheme according to the Charlson comorbidity
index based on ICD-8 (Brusselaers and Lagergren 2017). Assume that such
codes might be found in character strings with leading prefixes or in
the middle of a more verbatim description. This is controlled by setting
the argument `start = FALSE`, meaning that the identified ICD-8 codes do
not need to appear in the beginning of the character string. We might
assume, however, that there is no more information after the code (as
specified by `stop = TRUE`). We can also use some more specific and
unique group names as specified by `tech_names`.

``` r

charlson_icd8 <- 
  set_classcodes(
    "charlson",
    regex      = "icd8_brusselaers", # Version based on ICD-8
    start      = FALSE, # Codes do not have to occur in the beginning of a vector
    stop       = TRUE,  # Code vector must end with the specified codes
    tech_names = TRUE   # Use long but unique and descriptive variable names
  )
```

The resulting object has only one version of regular expressions
(`icd8_brusselaers` as specified). Each regular expression is suffixed
with `$` (due to `stop = TRUE`). Group names might seem cumbersome but
this will help to distinguish column names added by
[`categorize()`](https://docs.ropensci.org/coder/reference/categorize.md)
if this function is run repeatedly with different classcodes (i.e. if we
calculate both Charlson and Elixhauser indices for the same patients).
The original `charlson` object had 17 rows, but `charlson_icd8` has only
13, since not all groups are used in this version.

``` r

charlson_icd8
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
#>  1 charlson_icd8… Acute myoc… (41[0-2])$              1           1      1     1
#>  2 charlson_icd8… Heart fail… (4270|428)$             1           1      1     4
#>  3 charlson_icd8… Peripheral… (44[0-5])$              1           1      1     2
#>  4 charlson_icd8… Cerebrovas… (43[0-8])$              1           1      1     1
#>  5 charlson_icd8… Senile and… (290[01])$              1           1      1     0
#>  6 charlson_icd8… Chronic ob… (49[0-3]|51[5-8…        1           1      1     0
#>  7 charlson_icd8… Systemic l… (7(1[0-2]|34))$         1           1      1     0
#>  8 charlson_icd8… Paraplegia… (344)$                  2           1      1     0
#>  9 charlson_icd8… Chronic gl… (40[34]|58[0-3]…        2           1      1     3
#> 10 charlson_icd8… Diabetes w… (250)$                  2           1      1     0
#> 11 charlson_icd8… Malignant … (1([4-68][0-9]|…        2           1      1     0
#> 12 charlson_icd8… Hepatic co… (070|4560|51[1-…        3           1      1     0
#> 13 charlson_icd8… Secondary … (19[6-9])$              6           1      1     0
#> # ℹ 2 more variables: quan_original <dbl>, quan_updated <dbl>
```

Note that all index columns remain in the tibble. It is thus possible to
combine any categorization with any index, although some combinations
might be preferred (such as `regex_icd9cm_deyo` combined with
`index_deyo_ramano`).

We can now use `charlson_icd8` for classification:

``` r

classify(410, charlson_icd8)
#> Classification based on: icd8_brusselaers
#> 
#> The printed data is of class: classified, matrix.
#> It has 1 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 1 × 13
#>   charlson_icd8_brusselaers_myoc…¹ charlson_icd8_brusse…² charlson_icd8_brusse…³
#>   <lgl>                            <lgl>                  <lgl>                 
#> 1 TRUE                             FALSE                  FALSE                 
#> # ℹ abbreviated names: ¹​charlson_icd8_brusselaers_myocardial_infarction,
#> #   ²​charlson_icd8_brusselaers_congestive_heart_failure,
#> #   ³​charlson_icd8_brusselaers_peripheral_vascular_disease
#> # ℹ 10 more variables: charlson_icd8_brusselaers_cerebrovascular_disease <lgl>,
#> #   charlson_icd8_brusselaers_dementia <lgl>,
#> #   charlson_icd8_brusselaers_chronic_pulmonary_disease <lgl>,
#> #   charlson_icd8_brusselaers_rheumatic_disease <lgl>, …
```

The ICD-8 code `410`is recognized as (only) myocardial infarction.

### Implicit use of `set_classcodes()`

Instead of pre-specifying the `charlson_icd8`, a similar result is
achieved by:

``` r

classify(
  410,
  "charlson",
  cc_args = list(
    regex      = "icd8_brusselaers", 
    start      = FALSE, 
    stop       = TRUE,
    tech_names = TRUE
  )
)
#> 
#> The printed data is of class: classified, matrix.
#> It has 1 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 1 × 13
#>   charlson_icd8_brusselaers_myoc…¹ charlson_icd8_brusse…² charlson_icd8_brusse…³
#>   <lgl>                            <lgl>                  <lgl>                 
#> 1 TRUE                             FALSE                  FALSE                 
#> # ℹ abbreviated names: ¹​charlson_icd8_brusselaers_myocardial_infarction,
#> #   ²​charlson_icd8_brusselaers_congestive_heart_failure,
#> #   ³​charlson_icd8_brusselaers_peripheral_vascular_disease
#> # ℹ 10 more variables: charlson_icd8_brusselaers_cerebrovascular_disease <lgl>,
#> #   charlson_icd8_brusselaers_dementia <lgl>,
#> #   charlson_icd8_brusselaers_chronic_pulmonary_disease <lgl>,
#> #   charlson_icd8_brusselaers_rheumatic_disease <lgl>, …
```

## Bibliography

Brusselaers, Nele, and Jesper Lagergren. 2017. “The Charlson Comorbidity
Index in Registry-Based Research.” *Methods of Information in Medicine*
56 (05): 401–6. <https://doi.org/10.3414/ME17-01-0051>.
