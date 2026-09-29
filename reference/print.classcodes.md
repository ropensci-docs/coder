# Print classcodes object

Print classcodes object

## Usage

``` r
# S3 method for class 'classcodes'
print(x, n = NULL, ...)
```

## Arguments

- x:

  object of type classcodes

- n:

  number of rows to preview (`n = 0` is allowed)

- ...:

  arguments passed to print method for tibble

## See also

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

## Examples

``` r
# Default printing
elixhauser
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

# Print attributes data but no data preview
print(elixhauser, n = 0)
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
#> 

# Print all rows
print(elixhauser, n = 31)
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
#> 11 diabetes unc… E1[0… E1[0-4]     250[0… 250[0-3]|6480  250[0-3]              1
#> 12 diabetes com… E1[0… E1[0-4]     250[4… 250[4-9]|7751  250[4-9]              1
#> 13 hypothyroidi… E(0[… E(0[0-3]|8… 24(3|… 24(3|4[0-2]|4… 24(09|[34]|6[1…       1
#> 14 renal failure I(12… I(1[23])|N… 40(3(… 40(3([019]1)|… 40(3([019]1)|4…       1
#> 15 liver disease B18|… B18|I(8[56… 070(3… 070([23]{2}|[… 070([23]{2}|[4…       1
#> 16 peptic ulcer… K2[5… K2[5-8]     53([1… 53([1-4]([4-6… 53[1-4][79]           1
#> 17 AIDS/HIV      B2[0… B2[0-24]    04[2-… 04[2-4]        04[2-4]               1
#> 18 lymphoma      C(8[… C(8[1-58]|… 2(0([… 2(0([01]|2[0-… 2(0([0-2]|30)|…       1
#> 19 metastatic c… C(7[… C(7[7-9]|8… 19[6-… 19[6-9]        19[6-9]               1
#> 20 solid tumor   C([0… C([01]|2[0… 1([4-… 1([4-68]|7([0… 1([4-68]|7[0-2…       1
#> 21 rheumatoid a… L94[… L94|M(0[56… 7(010… 7(010|1[04]|2… 446|7(010|1(0[…       1
#> 22 coagulopathy  D6([… D6[5-9]     28(6|… 28(6|7[13-5])  28(6|7[13-5])         1
#> 23 obesity       E66   E66         2780   2780           2780                  1
#> 24 weight loss   E4[0… E4[0-6]|R6… 26[0-… 26[0-3]|7832   26[0-3]|7(832|…       1
#> 25 fluid electr… E(22… E(22|8[67]) 276    276            2(53|7)6              1
#> 26 blood loss a… D500  D50         2800   2800|6482      2800                  1
#> 27 deficiency a… D5(0… D5[0-3]     28(0[… 28(0[1-9]|1|5… 28(0[1-9]|1)          1
#> 28 alcohol abuse F10|… F10|E52|G6… 29(1[… 291[0-3589]|3… 2(652|9(1[1-35…       1
#> 29 drug abuse    F1[1… F1[1-689]|… 292(0… 292(0|8[2-9]|… 292|30(4|5[2-9…       1
#> 30 psychoses     F(2[… F(2[02-589… 29([5… 29([5-8]|91)   29(38|9([578]|…       1
#> 31 depression    F(20… F(20|3[1-4… 3(0(0… 3(0(04|112|9[… 29(6[235])|3(0…      NA
#> # ℹ 6 more variables: sum_all_ahrq <dbl>, walraven <dbl>, sid29 <dbl>,
#> #   sid30 <dbl>, ahrq_mort <dbl>, ahrq_readm <dbl>
```
