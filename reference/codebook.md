# codebook(s) for classcodes object

[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md)
and
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)
are used to summarize/visualize classcodes in R. A codebook, on the
other hand, is an exported summary saved in an Excel spreadsheet to use
in collaboration with non R-users. Several codebooks might be combined
into a single Excel document with several sheets (one for each
codebook).

## Usage

``` r
codebook(object, coding, ..., file = NULL)

# S3 method for class 'codebook'
print(x, ...)

codebooks(..., file = NULL)
```

## Arguments

- object:

  classcodes object

- coding:

  either a vector with codes from the original classification, or a name
  (character vector of length one) of a keyvalue object from package
  "decoder" (for example "icd10cm" or "atc")

- ...:

  Additional arguments for each function:

  - `codebook()`: arguments passed to
    [`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md)

  - `codebooks()`: multiple named outputs from `codebook()`

  - `print.codebook()`: arguments passed to `tibble:::print.tbl()`

- file:

  name/path to Excel file for data export

- x:

  output from `codebook()`

## Value

Functions are primarily called for their side effects (exporting data to
Excel or printing to screen). In addition:

- `codebook()`returns list of data frames describing relationship
  between groups and individual codes

- `codebooks()` returns a concatenated list with output from
  `codebook()`. Only one 'README' object is kept however and renamed as
  such.

- `print.codebook()`returns `x` (invisible)

## See also

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md),
[`visualize.classcodes()`](https://docs.ropensci.org/coder/reference/visualize.classcodes.md)

## Examples

``` r
# codebook() --------------------------------------------------------------
if (FALSE) { # \dontrun{
# Export codebook (to temporary file) with all codes identified by the
# Elixhauser comorbidity classification based on ICD-10-CM
codebook(elixhauser, "icd10cm", file = tempfile("codebook", fileext = ".xlsx"))

# All codes from ICD-9-CM Disease part used by Elixhauser enhanced version
codebook(elixhauser, "icd9cmd",
  cc_args = list(regex = "icd9cm_enhanced",
  file = tempfile("codebook", fileext = ".xlsx"))
)

# The codebook returns a list with three objects.
# Access a dictionary table with translates of each code to text:
codebook(charlson, "icd10cm")$all_codes


# print.codebook() --------------------------------------------------------

# If argument `file` is unspecified, a preview of each sheet of the codebook is
# printed to the screen
(cb <- codebook(charlson, "icd10cm"))

# The preview can be modified by arguments to the print-method
print(cb, n = 20)


# codebooks() -------------------------------------------------------------

# Combine codebooks based on different versions of the regular expressions
# and export to a single (temporary) Excel file
c1 <- codebook(elixhauser, "icd10cm")
c2 <- codebook(elixhauser, "icd9cmd",
  cc_args = list(regex = "icd9cm_enhanced")
  )

codebooks(
  elix_icd10 = c1, elix_icd9cm = c2,
  file = tempfile("codebooks", fileext = ".xlsx")
)
} # }
```
