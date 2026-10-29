# Example data

``` r

library(coder)
```

This vignette contains some example data used in the other vignettes.

## Patients

`ex_people` contains 100 patients (with random names from the
[`randomNames`](https://centerforassessment.github.io/randomNames/)
package) who received total hip arthroplasty (THA) surgery at given
(random) dates (`surgery` column). This data represent a sample from a
national quality register.

See also
[`?ex_people`](https://docs.ropensci.org/coder/reference/ex_people.md).

``` r

ex_people
#> # A tibble: 100 × 2
#>    name              surgery   
#>    <chr>             <date>    
#>  1 Chen, Trevor      2025-04-21
#>  2 Graves, Acineth   2025-01-11
#>  3 Trujillo, Yanelly 2024-12-29
#>  4 Simpson, Kenneth  2025-04-02
#>  5 Chin, Nelson      2025-03-16
#>  6 Le, Christina     2024-10-18
#>  7 Kang, Xuan        2025-01-20
#>  8 Shuemaker, Lauren 2024-10-19
#>  9 Boucher, Teresa   2025-03-27
#> 10 Le, Soraiya       2025-03-01
#> # ℹ 90 more rows
```

## Diagnoses data

We are interested in comorbidity for the patients above and have
collected some synthesized diagnostics data (`ex_icd10`) from a national
patient register (we can at least assume that for now). Patients have
one entry for every combination of recorded diagnoses codes according to
the International classification of diseases version 10, `icd10`, and
corresponding dates of hospital `admission`s for which those codes were
recorded. (Column `hdia` is `TRUE` for main diagnoses and `FALSE` for
underlying/less relevant codes).

See also
[`?ex_icd10`](https://docs.ropensci.org/coder/reference/ex_icd10.md).

``` r

ex_icd10
#> # A tibble: 2,376 × 4
#>    name                 admission  icd10 hdia 
#>    <chr>                <date>     <chr> <lgl>
#>  1 Tran, Kenneth        2024-11-02 S134A FALSE
#>  2 Tran, Kenneth        2025-04-18 W3319 FALSE
#>  3 Tran, Kenneth        2025-03-28 Y0262 TRUE 
#>  4 Tran, Kenneth        2025-02-18 X0488 FALSE
#>  5 Sommerville, Dominic 2025-04-09 V8104 FALSE
#>  6 Sommerville, Dominic 2024-11-18 B853  FALSE
#>  7 Sommerville, Dominic 2025-04-04 Q174  FALSE
#>  8 Sommerville, Dominic 2024-11-23 A227  FALSE
#>  9 Sommerville, Dominic 2025-03-30 H702  FALSE
#> 10 Sommerville, Dominic 2024-07-22 X6051 TRUE 
#> # ℹ 2,366 more rows
```

## Medical data

Assume we have some external code data from a national prescription
register. Such register would likely cover additional patients but let’s
just consider a small sample with ATC codes for patients above, such
that each patient can have zero, one, or several codes prescribed at
different dates.

``` r

ex_atc
#> # A tibble: 10,000 × 3
#>    name                 atc      prescription
#>    <chr>                <chr>    <date>      
#>  1 Le, Soraiya          L03AA16  2023-01-23  
#>  2 Cleveland, Mark      J07CA01  2020-10-02  
#>  3 Santistevan, Charlie QJ57EA06 2016-03-13  
#>  4 Meier, Hayden        R03DB04  2021-07-14  
#>  5 Hill, Audrey         V09IA01  2018-12-27  
#>  6 Thumma, Phillip      L02AE02  2015-02-04  
#>  7 Yost, Rebecca        S01EB06  2019-06-29  
#>  8 Mandakh, Joseph      A03DA01  2021-01-30  
#>  9 Meier, Hayden        C09AA13  2023-07-22  
#> 10 Trinh, Schuyler      A07EA03  2025-05-21  
#> # ℹ 9,990 more rows
```
