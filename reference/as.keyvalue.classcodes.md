# Make keyvalue object from classcodes object

S3-method for generic
[`decoder::as.keyvalue()`](https://eribul.github.io/decoder/reference/keyvalue.html)

## Usage

``` r
# S3 method for class 'classcodes'
as.keyvalue(x, coding, cc_args = list(), ...)
```

## Arguments

- x:

  classcodes object

- coding:

  either a vector with codes from the original classification, or a name
  (character vector of length one) of a keyvalue object from package
  "decoder" (for example "icd10cm" or "atc")

- cc_args:

  List of named arguments passed to
  [`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md)

- ...:

  additional arguments passed to
  [`decoder::as.keyvalue()`](https://eribul.github.io/decoder/reference/keyvalue.html)

## Value

Object of class `keyvalue` where `key` is the subset of codes from
`object$key`identified by the regular expression from `x` and where
`value` is the corresponding `x$group`. Hence, note that the original
`object$value` is not used in the output.

## Examples

``` r
# List all codes with corresponding classes as recognized by the Elixhauser
# comorbidity classification according to the Swedish version of the
# international classification of diseases version 10 (ICD-10-SE)
head(decoder::as.keyvalue(elixhauser, "icd10se"))
#> Classification based on: icd10
#> Error in derive_pubkey(key): RAW() can only be applied to a 'raw', not a 'character'

# Similar but with the American ICD-10-CM instead
# Note that the `value` column is similar as above
# (with names from `x$group`) and not
# from `object$value`
head(decoder::as.keyvalue(elixhauser, "icd10cm"))
#> Classification based on: icd10
#> Error in derive_pubkey(key): RAW() can only be applied to a 'raw', not a 'character'

# Codes identified by regular expressions based on ICD-9-CM and found in
# the Swedish version of ICD-9 used within the national cancer register
# (thus, a subset of the whole classification).
head(
  decoder::as.keyvalue(
    elixhauser, "icd9",
    cc_args = list(regex = "icd9cm")
  )
)
#> Error in derive_pubkey(key): RAW() can only be applied to a 'raw', not a 'character'
```
