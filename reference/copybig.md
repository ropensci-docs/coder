# Decide if large objects should be copied

Decide if large objects should be copied

## Usage

``` r
copybig(x, .copy = NA)
```

## Arguments

- x:

  object (potentially of large size)

- .copy:

  Should the object be copied internally by
  [`data.table::copy()`](https://rdrr.io/pkg/data.table/man/copy.html)?
  `NA` (by default) means that objects smaller than 1 GB are copied. If
  the size is larger, the argument must be set explicitly. Set `TRUE` to
  make copies regardless of object size. This is recommended if enough
  RAM is available. If set to `FALSE`, calculations might be carried out
  but the object will be changed by reference. IMPORTANT! This might
  lead to undesired consequences and should only be used if absolutely
  necessary!

## Value

Either `x` unchanged, or a fresh copy of `x`.
