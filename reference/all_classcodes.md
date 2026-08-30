# Summary data for all default classcodes object in the package

Tabulate object names and list all related versions of implemented
regular expressions and index weights.

## Usage

``` r
all_classcodes()
```

## Value

[tibble::tibble](https://tibble.tidyverse.org/reference/tibble.html)
with columns describing all default classcodes objects from the package.

## See also

Other classcodes:
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

## Examples

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
