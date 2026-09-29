# Classcodes for the comorbidity-polypharmacy score (CPS) based on ICD-10 codes

Classcodes for the comorbidity-polypharmacy score (CPS) based on ICD-10
codes

## Usage

``` r
cps
```

## Format

A data frame with 2 rows and 2 variables:

- group:

  comorbidity groups, either "ordinary" for most ICD-10-codes or
  "special" for codes beginning with "UA", "UB" and "UP"

- icd10:

  regular expressions identifying ICD-10 codes of each group

- only_ordinary:

  index weights, 1 for ordinary and 0 for special

## References

Stawicki, Stanislaw P., et al. "Comorbidity polypharmacy score and its
clinical utility: A pragmatic practitioner's perspective." Journal of
emergencies, trauma, and shock 8.4 (2015): 224.

## See also

Other default classcodes:
[`ae`](https://docs.ropensci.org/coder/reference/ae.md),
[`charlson`](https://docs.ropensci.org/coder/reference/charlson.md),
[`elixhauser`](https://docs.ropensci.org/coder/reference/elixhauser.md),
[`hip_ae_hailer`](https://docs.ropensci.org/coder/reference/hip_ae_hailer.md),
[`rxriskv`](https://docs.ropensci.org/coder/reference/rxriskv.md)
