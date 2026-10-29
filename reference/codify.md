# Codify case data with external code data (within specified time frames)

This is the first step of `codify() %>% classify() %>% index()`. The
function combines case data from one data set with related code data
from a second source, possibly limited to codes valid at certain time
points relative to case dates.

## Usage

``` r
codify(x, codedata, ..., id, code, date = NULL, code_date = NULL, days = NULL)

# S3 method for class 'data.frame'
codify(x, ..., id, date = NULL, days = NULL)

# S3 method for class 'data.table'
codify(
  x,
  codedata,
  ...,
  id,
  code,
  date = NULL,
  code_date = NULL,
  days = NULL,
  alnum = FALSE,
  .copy = NA
)

# S3 method for class 'codified'
print(x, ..., n = 10)
```

## Arguments

- x:

  data set with mandatory character id column (identified by argument
  `id = "<col_name>"`), and optional
  [`Date`](https://rdrr.io/r/base/Dates.html) of interest (identified by
  argument `date = "<col_name>"`). Alternatively, the output from
  `codify()`

- codedata:

  additional data with columns including case id (`character`), code and
  an optional date ([Date](https://rdrr.io/r/base/Dates.html)) for each
  code. An optional column `condition` might distinguish codes/dates
  with certain characteristics (see example).

- ...:

  arguments passed between methods

- id, code, date, code_date:

  column names with case id (`character` from `x` and `codedata`),
  `code` (from `x`) and optional date
  ([Date](https://rdrr.io/r/base/Dates.html) from `x`) and `code_date`
  ([Date](https://rdrr.io/r/base/Dates.html) from `codedata`).

- days:

  numeric vector of length two with lower and upper bound for range of
  relevant days relative to `date`. See "Relevant period".

- alnum:

  Should codes be cleaned from all non alphanumeric characters?

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

- n:

  number of rows to preview as tibble. The output is technically a
  [data.table::data.table](https://rdrr.io/pkg/data.table/man/data.table.html),
  which might be an unusual format to look at. Use `n = NULL` to print
  the object as is.

## Value

Object of class `codified` (inheriting from
[data.table::data.table](https://rdrr.io/pkg/data.table/man/data.table.html)).
Essentially `x` with additional columns: `code, code_date`: left joined
from `codedata` or `NA` if no match within period. `in_period`: Boolean
indicator if the case had at least one code within the specified period.

The output has one row for each combination of "id" from `x` and "code"
from `codedata`. Rows from `x` might be repeated accordingly.

## Relevant period

Some examples for argument `days`:

- `c(-365, -1)`: window of one year prior to the `date` column of `x`.
  Useful for patient comorbidity.

- `c(1, 30)`: window of 30 days after `date`. Useful for adverse events
  after a surgical procedure.

- `c(-Inf, Inf)`: no limitation on non-missing dates.

- `NULL`: no time limitation at all.

## See also

Other verbs:
[`categorize()`](https://docs.ropensci.org/coder/reference/categorize.md),
[`classify()`](https://docs.ropensci.org/coder/reference/classify.md),
[`index_fun`](https://docs.ropensci.org/coder/reference/index_fun.md)

## Examples

``` r
# Codify all patients from `ex_people` with their ICD-10 codes from `ex_icd10`
x <- codify(ex_people, ex_icd10, id = "name", code = "icd10")
x
#> 
#> The printed data is of class: codified, data.table, data.frame.
#> It has 700 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 6
#>    name                admission  icd10 hdia  in_period surgery   
#>    <chr>               <date>     <chr> <lgl> <lgl>     <date>    
#>  1 Archer, Leon Hunter 2025-02-17 B469  FALSE TRUE      2024-12-15
#>  2 Archer, Leon Hunter 2024-06-27 E012  FALSE TRUE      2024-12-15
#>  3 Archer, Leon Hunter 2025-03-06 R900  FALSE TRUE      2024-12-15
#>  4 Archer, Leon Hunter 2024-09-02 V7413 FALSE TRUE      2024-12-15
#>  5 Archer, Leon Hunter 2024-07-19 V8698 FALSE TRUE      2024-12-15
#>  6 Archer, Leon Hunter 2024-08-09 X3403 FALSE TRUE      2024-12-15
#>  7 Archer, Leon Hunter 2024-08-08 X4128 FALSE TRUE      2024-12-15
#>  8 Archer, Leon Hunter 2024-08-25 Z752  FALSE TRUE      2024-12-15
#>  9 Awtrey, Antonio     2025-04-17 N608  FALSE TRUE      2025-04-12
#> 10 Awtrey, Antonio     2025-02-17 W0341 FALSE TRUE      2025-04-12

# Only consider codes if recorded at hospital admissions within one year prior
# to surgery
codify(
  ex_people,
  ex_icd10,
  id        = "name",
  code      = "icd10",
  date      = "surgery",
  code_date = "admission",
  days      = c(-365, 0)   # admission during one year before surgery
)
#> 
#> The printed data is of class: codified, data.table, data.frame.
#> It has 378 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 6
#>    name                surgery    admission  icd10 hdia  in_period
#>    <chr>               <date>     <date>     <chr> <lgl> <lgl>    
#>  1 Archer, Leon Hunter 2024-12-15 2024-06-27 E012  FALSE TRUE     
#>  2 Archer, Leon Hunter 2024-12-15 2024-07-19 V8698 FALSE TRUE     
#>  3 Archer, Leon Hunter 2024-12-15 2024-08-08 X4128 FALSE TRUE     
#>  4 Archer, Leon Hunter 2024-12-15 2024-08-09 X3403 FALSE TRUE     
#>  5 Archer, Leon Hunter 2024-12-15 2024-08-25 Z752  FALSE TRUE     
#>  6 Archer, Leon Hunter 2024-12-15 2024-09-02 V7413 FALSE TRUE     
#>  7 Awtrey, Antonio     2025-04-12 2024-08-05 X3322 FALSE TRUE     
#>  8 Awtrey, Antonio     2025-04-12 2024-10-26 Y1614 FALSE TRUE     
#>  9 Awtrey, Antonio     2025-04-12 2024-10-29 X7564 FALSE TRUE     
#> 10 Awtrey, Antonio     2025-04-12 2024-12-16 X6542 FALSE TRUE     

# Only consider codes if recorded after surgery
codify(
  ex_people,
  ex_icd10,
  id        = "name",
  code      = "icd10",
  date      = "surgery",
  code_date = "admission",
  days      = c(1, Inf)     # admission any time after surgery
)
#> 
#> The printed data is of class: codified, data.table, data.frame.
#> It has 355 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 6
#>    name                surgery    admission  icd10 hdia  in_period
#>    <chr>               <date>     <date>     <chr> <lgl> <lgl>    
#>  1 Archer, Leon Hunter 2024-12-15 2025-02-17 B469  FALSE TRUE     
#>  2 Archer, Leon Hunter 2024-12-15 2025-03-06 R900  FALSE TRUE     
#>  3 Awtrey, Antonio     2025-04-12 2025-04-17 N608  FALSE TRUE     
#>  4 Bammesberger, Jozi  2024-10-15 2024-11-18 V4931 FALSE TRUE     
#>  5 Bammesberger, Jozi  2024-10-15 2024-12-02 V4960 FALSE TRUE     
#>  6 Bammesberger, Jozi  2024-10-15 2025-01-22 X6414 FALSE TRUE     
#>  7 Bammesberger, Jozi  2024-10-15 2025-02-10 Y1513 FALSE TRUE     
#>  8 Bammesberger, Jozi  2024-10-15 2025-05-01 P293A FALSE TRUE     
#>  9 Banks, Silbret      2025-01-08 2025-01-26 D229D FALSE TRUE     
#> 10 Banks, Silbret      2025-01-08 2025-02-15 V7452 TRUE  TRUE     


# Dirty code data ---------------------------------------------------------

# Assume that codes contain unwanted "dirty" characters
# Those could for example be a dot used by ICD-10 (i.e. X12.3 instead of X123)
dirt <- c(strsplit(c("!#%&/()=?`,.-_"), split = ""), recursive = TRUE)
rdirt <- function(x) sample(x, nrow(ex_icd10), replace = TRUE)
sub <- function(i) substr(ex_icd10$icd10, i, i)
ex_icd10$icd10 <-
  paste0(
    rdirt(dirt), sub(1),
    rdirt(dirt), sub(2),
    rdirt(dirt), sub(3),
    rdirt(dirt), sub(4),
    rdirt(dirt), sub(5)
  )
head(ex_icd10)
#> # A tibble: 6 × 4
#>   name                 admission  icd10      hdia 
#>   <chr>                <date>     <chr>      <lgl>
#> 1 Tran, Kenneth        2024-11-02 -S_1_3,4.A FALSE
#> 2 Tran, Kenneth        2025-04-18 )W/3(3&1)9 FALSE
#> 3 Tran, Kenneth        2025-03-28 .Y_0%2?6?2 TRUE 
#> 4 Tran, Kenneth        2025-02-18 /X-0/4%8-8 FALSE
#> 5 Sommerville, Dominic 2025-04-09 (V!8-1)0?4 FALSE
#> 6 Sommerville, Dominic 2024-11-18 &B=8_5%3-  FALSE

# Use `alnum = TRUE` to ignore non alphanumeric characters
codify(ex_people, ex_icd10, id = "name", code = "icd10", alnum = TRUE)
#> 
#> The printed data is of class: codified, data.table, data.frame.
#> It has 700 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 6
#>    name                admission  icd10 hdia  in_period surgery   
#>    <chr>               <date>     <chr> <lgl> <lgl>     <date>    
#>  1 Archer, Leon Hunter 2024-08-09 X3403 FALSE TRUE      2024-12-15
#>  2 Archer, Leon Hunter 2024-07-19 V8698 FALSE TRUE      2024-12-15
#>  3 Archer, Leon Hunter 2025-03-06 R900  FALSE TRUE      2024-12-15
#>  4 Archer, Leon Hunter 2024-08-25 Z752  FALSE TRUE      2024-12-15
#>  5 Archer, Leon Hunter 2025-02-17 B469  FALSE TRUE      2024-12-15
#>  6 Archer, Leon Hunter 2024-08-08 X4128 FALSE TRUE      2024-12-15
#>  7 Archer, Leon Hunter 2024-06-27 E012  FALSE TRUE      2024-12-15
#>  8 Archer, Leon Hunter 2024-09-02 V7413 FALSE TRUE      2024-12-15
#>  9 Awtrey, Antonio     2025-04-17 N608  FALSE TRUE      2025-04-12
#> 10 Awtrey, Antonio     2025-02-17 W0341 FALSE TRUE      2025-04-12



# Big data ----------------------------------------------------------------

# If `data` or `codedata` are large compared to available
# Random Access Memory (RAM) it might not be possible to make internal copies
# of those objects. Setting `.copy = FALSE` might help to overcome such problems

# If no copies are made internally, however, the input objects (if data tables)
# would change in the global environment
x2 <- data.table::as.data.table(ex_icd10)
head(x2) # Look at the "icd10" column (with dirty data)
#>                    name  admission      icd10   hdia
#>                  <char>     <Date>     <char> <lgcl>
#> 1:        Tran, Kenneth 2024-11-02 -S_1_3,4.A  FALSE
#> 2:        Tran, Kenneth 2025-04-18 )W/3(3&1)9  FALSE
#> 3:        Tran, Kenneth 2025-03-28 .Y_0%2?6?2   TRUE
#> 4:        Tran, Kenneth 2025-02-18 /X-0/4%8-8  FALSE
#> 5: Sommerville, Dominic 2025-04-09 (V!8-1)0?4  FALSE
#> 6: Sommerville, Dominic 2024-11-18  &B=8_5%3-  FALSE

# Use `alnum = TRUE` combined with `.copy = FALSE`
codify(ex_people, x2, id = "name", code = "icd10", alnum = TRUE, .copy = FALSE)
#> 
#> The printed data is of class: codified, data.table, data.frame.
#> It has 700 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 6
#>    name                admission  icd10 hdia  in_period surgery   
#>    <chr>               <date>     <chr> <lgl> <lgl>     <date>    
#>  1 Archer, Leon Hunter 2025-03-06 R900  FALSE TRUE      2024-12-15
#>  2 Archer, Leon Hunter 2024-08-09 X3403 FALSE TRUE      2024-12-15
#>  3 Archer, Leon Hunter 2024-08-08 X4128 FALSE TRUE      2024-12-15
#>  4 Archer, Leon Hunter 2024-08-25 Z752  FALSE TRUE      2024-12-15
#>  5 Archer, Leon Hunter 2024-06-27 E012  FALSE TRUE      2024-12-15
#>  6 Archer, Leon Hunter 2024-09-02 V7413 FALSE TRUE      2024-12-15
#>  7 Archer, Leon Hunter 2024-07-19 V8698 FALSE TRUE      2024-12-15
#>  8 Archer, Leon Hunter 2025-02-17 B469  FALSE TRUE      2024-12-15
#>  9 Awtrey, Antonio     2024-12-16 X6542 FALSE TRUE      2025-04-12
#> 10 Awtrey, Antonio     2025-03-20 X4078 FALSE TRUE      2025-04-12

# Even though no explicit assignment was specified
# (neither for the output of codify(), nor to explicitly alter `x2`,
# the `x2` object has changed (look at the "icd10" column!):
head(x2)
#>                    name  admission  icd10   hdia
#>                  <char>     <Date> <char> <lgcl>
#> 1:        Tran, Kenneth 2024-11-02  S134A  FALSE
#> 2:        Tran, Kenneth 2025-04-18  W3319  FALSE
#> 3:        Tran, Kenneth 2025-03-28  Y0262   TRUE
#> 4:        Tran, Kenneth 2025-02-18  X0488  FALSE
#> 5: Sommerville, Dominic 2025-04-09  V8104  FALSE
#> 6: Sommerville, Dominic 2024-11-18   B853  FALSE

# Hence, the `.copy` argument should only be used if necessary
# and if so, with caution!


# print.codify() ----------------------------------------------------------

x # Preview first 10 rows as a tibble
#> 
#> The printed data is of class: codified, data.table, data.frame.
#> It has 700 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 6
#>    name                admission  icd10 hdia  in_period surgery   
#>    <chr>               <date>     <chr> <lgl> <lgl>     <date>    
#>  1 Archer, Leon Hunter 2025-02-17 B469  FALSE TRUE      2024-12-15
#>  2 Archer, Leon Hunter 2024-06-27 E012  FALSE TRUE      2024-12-15
#>  3 Archer, Leon Hunter 2025-03-06 R900  FALSE TRUE      2024-12-15
#>  4 Archer, Leon Hunter 2024-09-02 V7413 FALSE TRUE      2024-12-15
#>  5 Archer, Leon Hunter 2024-07-19 V8698 FALSE TRUE      2024-12-15
#>  6 Archer, Leon Hunter 2024-08-09 X3403 FALSE TRUE      2024-12-15
#>  7 Archer, Leon Hunter 2024-08-08 X4128 FALSE TRUE      2024-12-15
#>  8 Archer, Leon Hunter 2024-08-25 Z752  FALSE TRUE      2024-12-15
#>  9 Awtrey, Antonio     2025-04-17 N608  FALSE TRUE      2025-04-12
#> 10 Awtrey, Antonio     2025-02-17 W0341 FALSE TRUE      2025-04-12
print(x, n = 20) # Preview first 20 rows as a tibble
#> 
#> The printed data is of class: codified, data.table, data.frame.
#> It has 700 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 20 × 6
#>    name                admission  icd10 hdia  in_period surgery   
#>    <chr>               <date>     <chr> <lgl> <lgl>     <date>    
#>  1 Archer, Leon Hunter 2025-02-17 B469  FALSE TRUE      2024-12-15
#>  2 Archer, Leon Hunter 2024-06-27 E012  FALSE TRUE      2024-12-15
#>  3 Archer, Leon Hunter 2025-03-06 R900  FALSE TRUE      2024-12-15
#>  4 Archer, Leon Hunter 2024-09-02 V7413 FALSE TRUE      2024-12-15
#>  5 Archer, Leon Hunter 2024-07-19 V8698 FALSE TRUE      2024-12-15
#>  6 Archer, Leon Hunter 2024-08-09 X3403 FALSE TRUE      2024-12-15
#>  7 Archer, Leon Hunter 2024-08-08 X4128 FALSE TRUE      2024-12-15
#>  8 Archer, Leon Hunter 2024-08-25 Z752  FALSE TRUE      2024-12-15
#>  9 Awtrey, Antonio     2025-04-17 N608  FALSE TRUE      2025-04-12
#> 10 Awtrey, Antonio     2025-02-17 W0341 FALSE TRUE      2025-04-12
#> 11 Awtrey, Antonio     2024-08-05 X3322 FALSE TRUE      2025-04-12
#> 12 Awtrey, Antonio     2025-03-20 X4078 FALSE TRUE      2025-04-12
#> 13 Awtrey, Antonio     2024-12-16 X6542 FALSE TRUE      2025-04-12
#> 14 Awtrey, Antonio     2024-10-29 X7564 FALSE TRUE      2025-04-12
#> 15 Awtrey, Antonio     2025-02-11 Y0492 FALSE TRUE      2025-04-12
#> 16 Awtrey, Antonio     2024-10-26 Y1614 FALSE TRUE      2025-04-12
#> 17 Bammesberger, Jozi  2025-05-01 P293A FALSE TRUE      2024-10-15
#> 18 Bammesberger, Jozi  2024-06-12 V1051 FALSE TRUE      2024-10-15
#> 19 Bammesberger, Jozi  2024-08-19 V1392 FALSE TRUE      2024-10-15
#> 20 Bammesberger, Jozi  2024-11-18 V4931 FALSE TRUE      2024-10-15
print(x, n = NULL) # Print as data.table (ignoring the 'classified' class)
#> 
#> The printed data is of class: codified, data.table, data.frame.
#> It has 700 row(s).
#> It is here previewed as a tibble
#> Use `print(x, n = NULL)` to print as is (or use `n` to specify the number of rows to preview)!
#> 
#> # A tibble: 10 × 6
#>    name                admission  icd10 hdia  in_period surgery   
#>    <chr>               <date>     <chr> <lgl> <lgl>     <date>    
#>  1 Archer, Leon Hunter 2025-02-17 B469  FALSE TRUE      2024-12-15
#>  2 Archer, Leon Hunter 2024-06-27 E012  FALSE TRUE      2024-12-15
#>  3 Archer, Leon Hunter 2025-03-06 R900  FALSE TRUE      2024-12-15
#>  4 Archer, Leon Hunter 2024-09-02 V7413 FALSE TRUE      2024-12-15
#>  5 Archer, Leon Hunter 2024-07-19 V8698 FALSE TRUE      2024-12-15
#>  6 Archer, Leon Hunter 2024-08-09 X3403 FALSE TRUE      2024-12-15
#>  7 Archer, Leon Hunter 2024-08-08 X4128 FALSE TRUE      2024-12-15
#>  8 Archer, Leon Hunter 2024-08-25 Z752  FALSE TRUE      2024-12-15
#>  9 Awtrey, Antonio     2025-04-17 N608  FALSE TRUE      2025-04-12
#> 10 Awtrey, Antonio     2025-02-17 W0341 FALSE TRUE      2025-04-12
```
