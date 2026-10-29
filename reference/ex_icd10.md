# Example data for random codes assigned to random people

Example data for fictive ICD-10-diagnoses to use for testing and in
examples.

## Usage

``` r
ex_icd10
```

## Format

Data frames with 1,000 rows and 4 variables:

- id:

  Random names corresponding to column `name` in dataset `ex_people`

- date:

  random dates corresponding to registered (comorbidity) codes

- code:

  ICD-10 codes from the `uranium_pathology` dataset in the `icd.data`
  package by Jack Wasey originating from the United States Transuranium
  and Uranium Registries, published in the public domain.

- hdia:

  boolean marker if corresponding code is the main diagnose of the
  hospital visit (randomly assigned to 10 percent of the codes)

## Source

https://github.com/jackwasey/icd.data https://ustur.wsu.edu/about-us/

## See also

Other example data:
[`ex_atc`](https://docs.ropensci.org/coder/reference/ex_atc.md),
[`ex_people`](https://docs.ropensci.org/coder/reference/ex_people.md)
