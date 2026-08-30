# Visualize classification scheme in web browser

Groups from a `classcodes` object are visualized by their regular
expressions in the default web browser. The visualization does not give
any details on group names, conditions or weights but might be useful
both for understanding of a classification scheme in use, and during the
creation and debugging of such.

## Usage

``` r
# S3 method for class 'classcodes'
visualize(x, group = NULL, show = TRUE, ...)
```

## Arguments

- x:

  [classcodes](https://docs.ropensci.org/coder/reference/classcodes.md)
  object or name of such object included in the package (see
  [`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md)).

- group:

  names (as character vector) of groups to visualize (subset of
  `rownames(x)`). (All groups if `NULL`.)

- show:

  should a visualization be shown in the default web browser. Set to
  `FALSE` to just retrieve a URL for later use.

- ...:

  Arguments passed on to
  [`set_classcodes`](https://docs.ropensci.org/coder/reference/set_classcodes.md)

  `regex`

  :   name of column with regular expressions to use for classification.
      `NULL` (default) uses `attr(obj, "regexpr")[1]`.

## Value

URL to website with visualization (invisible)

## See also

Other classcodes:
[`all_classcodes()`](https://docs.ropensci.org/coder/reference/all_classcodes.md),
[`as.data.frame.classified()`](https://docs.ropensci.org/coder/reference/as.data.frame.classified.md),
[`classcodes`](https://docs.ropensci.org/coder/reference/classcodes.md),
[`codebook()`](https://docs.ropensci.org/coder/reference/codebook.md),
[`print.classcodes()`](https://docs.ropensci.org/coder/reference/print.classcodes.md),
[`print.classified()`](https://docs.ropensci.org/coder/reference/print.classified.md),
[`set_classcodes()`](https://docs.ropensci.org/coder/reference/set_classcodes.md),
[`summary.classcodes()`](https://docs.ropensci.org/coder/reference/summary.classcodes.md)

## Examples

``` r

# The default behavior is to open a visualization in the default web browser
if (FALSE) { # \dontrun{

 # How is depression classified according to Elixhauser?
 visualize("elixhauser", "depression")

 # Compare the two diabetes groups according to Charlson
 visualize("charlson",
   c("diabetes without complication", "diabetes complication"))

 # Is this different from the "Royal College of Surgeons classification?
 # Yes, there is only one group for diabetes
 visualize("charlson",
   c("diabetes without complication", "diabetes complication"),
   regex = "rcs"
 )

 # Show all groups from Charlson
 visualize("charlson")

 # It is also possible to visualize an arbitrary regular expression
 # from a character string
 visualize("I2([12]|52)")
} # }

 # The URL is always returned (invisable) but the visual display can
 # also be omitted
url <- visualize("hip_ae", show = FALSE)
#> Classification based on: icd10
url
#> [1] "https://jex.im/regulex/#!embed=true&flags=&re=%5E(%5E(G(97[89])%7CM96(6F%7C[89])%7C(T8((1([02-9]%7C8W))%7C(4([03578]F?)%7C[49])%7C(8[89])))))%7C%5E(%5E(G57[0-2]%7CM((00(0F?%7C[289]F))%7C(24(3%7C4F?)))%7CS7([4-6]%7C30)))%7C%5E(%5E(M(24[056]F%7C6(10%7C21%7C6[23])F%7C8((43%7C6[01])F%7C66F?%7C95E))))%7C%5E(%5E(I((2([14]%7C6[09]))%7C4(6[019]%7C90)%7C6([0-35-6]%7C49)%7C7([24]%7C7[0-2])%7C8(19%7C2)%7C97[89])%7CJ8[01]9%7CT811))%7C%5E(%5E(I80%7CJ((1[3-8])%7C9(5[23589]%7C6%7C81))%7CK2[5-7]%7CL89%7CN(99[089]%7C(17))%7CR339))%7C%5E(%5E(J2[0-2]%7CK(590%7C29)%7CN991))"
```
