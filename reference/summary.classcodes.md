# Summarizing a classcodes object

Classification schemes are formalized by regular expressions within the
classcodes objects. These are computationally effective but sometimes
hard to interpret. Use this function to list all codes identified for
each group.

## Usage

``` r
# S3 method for class 'classcodes'
summary(object, coding, ..., cc_args = list())

# S3 method for class 'summary.classcodes'
print(x, ...)
```

## Arguments

- object:

  classcodes object

- coding:

  either a vector with codes from the original classification, or a name
  (character vector of length one) of a keyvalue object from package
  "decoder" (for example "icd10cm" or "atc")

- ...:

  - `summary.classcodes()`: ignored

  - `print.summary.classcodes()`: arguments passed to
    `tibble:::print.tbl()`

- cc_args:

  List of named arguments passed to
  [`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md)

- x:

  output from `summary.classcodes()`

## Value

Methods primarily called for their side effects (printing to the screen)
but with additional invisible objects returned:

- `summary.classcodes()`: list with input arguments `object` and
  `coding` unchanged, as well as a data frame (`summary`) with columns
  for groups identified (`group`); the number of codes to be recognized
  for each group (`n`) and individual codes within each group (`codes`).

- `print.summary.classcodes()`: argument `x` unchanged

## See also

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

## Examples

``` r

# summary.classcodes() ----------------------------------------------------

# Summarize all ICD-10-CM codes identified by the Elixhauser
# comorbidity classification
# See `?decoder::icd10cm` for details
summary(elixhauser, coding = "icd10cm")
#> Classification based on: icd10
#> 
#> Summary of classcodes object
#> 
#> Recognized codes per group:
#> 
#> # A tibble: 31 × 3
#>    group                         n codes                                        
#>    <chr>                     <int> <chr>                                        
#>  1 AIDS/HIV                      1 B20                                          
#>  2 alcohol abuse               139 E52, F1010, F1011, F10120, F10121, F10129, F…
#>  3 blood loss anemia             1 D500                                         
#>  4 cardiac arrhythmias          76 I441, I442, I4430, I4439, I456, I459, I470, …
#>  5 chronic pulmonary disease    75 I27812, I27822, I27832, I278402, I278412, I2…
#>  6 coagulopathy                 37 D65, D66, D67, D680, D6800, D6801, D68020, D…
#>  7 congestive heart failure     36 I099, I1101, I1301, I1321, I255, I420, I425,…
#>  8 deficiency anemia            17 D508, D509, D510, D511, D512, D513, D518, D5…
#>  9 depression                   33 F3130, F3131, F3132, F314, F3152, F320, F321…
#> 10 diabetes complicated        272 E1021, E1022, E1029, E10311, E10319, E10321,…
#> # ℹ 21 more rows
#> 
#>  Use function visualize() for a graphical representation.

# Is there a difference if instead considering the Swedish ICD-10-SE?
# See `?decoder::icd10se` for details
summary(elixhauser, coding = "icd10se")
#> Classification based on: icd10
#> 
#> Summary of classcodes object
#> 
#> Recognized codes per group:
#> 
#> # A tibble: 31 × 3
#>    group                         n codes                                        
#>    <chr>                     <int> <chr>                                        
#>  1 AIDS/HIV                     26 B20, B200, B201, B202, B203, B204, B205, B20…
#>  2 alcohol abuse                37 E52, E529, F10, F100, F101, F102, F102A, F10…
#>  3 blood loss anemia             1 D500                                         
#>  4 cardiac arrhythmias          48 I441, I441A, I441B, I442, I443, I456, I456A,…
#>  5 chronic pulmonary disease    73 I2782, I2792, J40, J409, J41, J410, J411, J4…
#>  6 coagulopathy                 33 D65, D659, D66, D669, D67, D679, D68, D680, …
#>  7 congestive heart failure     25 I099, I1101, I1301, I1321, I255, I420, I425,…
#>  8 deficiency anemia            20 D508, D509, D51, D510, D511, D512, D513, D51…
#>  9 depression                   24 F2042, F313, F314, F3152, F32, F320, F321, F…
#> 10 diabetes complicated         93 E102, E102A, E102B, E102C, E102W, E102X, E10…
#> # ℹ 21 more rows
#> 
#>  Use function visualize() for a graphical representation.

# Which ICD-9-CM diagnostics codes are recognized by Charlson according to
# Brusselears et al. 2017 (see `?charlson`)
summary(
  charlson, coding = "icd9cmd",
  cc_args = list(regex = "icd9_brusselaers")
)
#> 
#> Summary of classcodes object
#> 
#> Recognized codes per group:
#> 
#> # A tibble: 13 × 3
#>    group                             n codes                                    
#>    <chr>                         <int> <chr>                                    
#>  1 cerebrovascular disease          69 430, 431, 4320, 4321, 4329, 43300, 43301…
#>  2 chronic pulmonary disease        48 4160, 4161, 4162, 4168, 4169, 490, 4910,…
#>  3 congestive heart failure         45 40200, 40201, 40210, 40211, 40290, 40291…
#>  4 dementia                         21 2900, 29010, 29011, 29012, 29013, 29020,…
#>  5 diabetes complication            31 1960, 1961, 1962, 1963, 1965, 1966, 1968…
#>  6 diabetes without complication    40 25000, 25001, 25002, 25003, 25010, 25011…
#>  7 hemiplegia or paraplegia        628 1400, 1401, 1403, 1404, 1405, 1406, 1408…
#>  8 mild liver disease               67 40300, 40301, 40310, 40311, 40390, 40391…
#>  9 myocardial infarction            31 41000, 41001, 41002, 41010, 41011, 41012…
#> 10 peptic ulcer disease             39 34200, 34201, 34202, 34210, 34211, 34212…
#> 11 peripheral vascular disease      82 4400, 4401, 44020, 44021, 44022, 44023, …
#> 12 renal disease                    50 0700, 0701, 07020, 07021, 07022, 07023, …
#> 13 rheumatic disease               179 7100, 7101, 7102, 7103, 7104, 7105, 7108…
#> 
#>  Use function visualize() for a graphical representation.


# print.summary.classcodes() ----------------------------------------------

# Print all 31 lines of the summarized Elixhauser classcodes object
print(
  summary(elixhauser, coding = "icd10cm"),
  n = 31
)
#> Classification based on: icd10
#> 
#> Summary of classcodes object
#> 
#> Recognized codes per group:
#> 
#> # A tibble: 31 × 3
#>    group                              n codes                                   
#>    <chr>                          <int> <chr>                                   
#>  1 AIDS/HIV                           1 B20                                     
#>  2 alcohol abuse                    139 E52, F1010, F1011, F10120, F10121, F101…
#>  3 blood loss anemia                  1 D500                                    
#>  4 cardiac arrhythmias               76 I441, I442, I4430, I4439, I456, I459, I…
#>  5 chronic pulmonary disease         75 I27812, I27822, I27832, I278402, I27841…
#>  6 coagulopathy                      37 D65, D66, D67, D680, D6800, D6801, D680…
#>  7 congestive heart failure          36 I099, I1101, I1301, I1321, I255, I420, …
#>  8 deficiency anemia                 17 D508, D509, D510, D511, D512, D513, D51…
#>  9 depression                        33 F3130, F3131, F3132, F314, F3152, F320,…
#> 10 diabetes complicated             272 E1021, E1022, E1029, E10311, E10319, E1…
#> 11 diabetes uncomplicated            13 E1010, E1011, E109, E1100, E1101, E1110…
#> 12 drug abuse                       380 F1110, F1111, F11120, F11121, F11122, F…
#> 13 fluid electrolyte disorders       19 E222, E860, E861, E869, E870, E871, E87…
#> 14 hypertension complicated          13 I1102, I119, I1201, I129, I1302, I13101…
#> 15 hypertension uncomplicated         1 I10                                     
#> 16 hypothyroidism                    18 E000, E001, E002, E009, E010, E011, E01…
#> 17 liver disease                     59 B180, B181, B182, B188, B189, I8500, I8…
#> 18 lymphoma                         424 C8100, C8101, C8102, C8103, C8104, C810…
#> 19 metastatic cancer                 48 C770, C771, C772, C773, C774, C775, C77…
#> 20 obesity                           11 E6601, E6609, E661, E662, E663, E668, E…
#> 21 other neurological disorders     153 G10, G110, G111, G1110, G1111, G1119, G…
#> 22 paralysis                         45 G041, G1141, G801, G802, G8100, G8101, …
#> 23 peptic ulcer disease               8 K257, K259, K267, K269, K277, K279, K28…
#> 24 peripheral vascular disorder     293 I700, I701, I70201, I70202, I70203, I70…
#> 25 psychoses                         20 F200, F201, F202, F203, F205, F2081, F2…
#> 26 pulmonary circulation disorder    33 I2601, I2602, I2603, I2604, I2609, I269…
#> 27 renal failure                     15 I1202, I13102, I13112, N181, N182, N183…
#> 28 rheumatoid arthritis             601 L940, L941, L943, M0500, M05011, M05012…
#> 29 solid tumor                      491 C000, C001, C002, C003, C004, C005, C00…
#> 30 valvular disease                  60 A5200, A5201, A5202, A5203, A5204, A520…
#> 31 weight loss                       10 E40, E41, E42, E43, E440, E441, E45, E4…
#> 
#>  Use function visualize() for a graphical representation.
```
