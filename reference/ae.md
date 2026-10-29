# Classcodes for adverse events after knee and hip arthroplasty

ICD-10 group names are prefixed by two letters as given by the
references. Two groups (DB and DM) are split into two due to different
conditions.

## Usage

``` r
knee_ae

hip_ae
```

## Format

Data frame with 3 columns:

- group:

  Different types of adverse events (see reference section)

- icd10:

  regular expressions identifying ICD-10 codes for each group

- icd10_fracture:

  regular expressions for fracture patients. Essentially the same as
  `regex` but with some additional codes for group "DM1 other"

- kva:

  regular expressions identifying KVA codes

- condition:

  special conditions are used, see below.

An object of class `classcodes` (inherits from `tbl_df`, `tbl`,
`data.frame`) with 7 rows and 5 columns.

## Hip fractures

Adverse events (AE) codes for hip fractures are based on codes for
elective cases but with some additional codes for DM 1 (N300, N308, N309
and N390).

## Conditions

Special conditions apply to all categories. Those require non-standard
modifications of the classcodes data prior to categorization.

- hbdia1_hdia:

  `TRUE` if the code was given as any type of diagnose during hospital
  visit for index operation, or as main diagnose for later visits,
  otherwise `FALSE`

- late_hdia:

  `TRUE` if the code was given as main diagnose at a later visit after
  the index operation, otherwise `FALSE`

- post_op:

  `TRUE` if the code was given at a later visit after the index
  operation, otherwise `FALSE`

## References

Magneli M, Unbeck M, Rogmark C, Rolfson O, Hommel A, Samuelsson B, et
al. Validation of adverse events after hip arthroplasty: a Swedish
multi-centre cohort study. BMJ Open. 2019 Mar 7;9(3):e023773.

## See also

hip_ae_hailer

Other default classcodes:
[`charlson`](https://docs.ropensci.org/coder/reference/charlson.md),
[`cps`](https://docs.ropensci.org/coder/reference/cps.md),
[`elixhauser`](https://docs.ropensci.org/coder/reference/elixhauser.md),
[`hip_ae_hailer`](https://docs.ropensci.org/coder/reference/hip_ae_hailer.md),
[`rxriskv`](https://docs.ropensci.org/coder/reference/rxriskv.md)
