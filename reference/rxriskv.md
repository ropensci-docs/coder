# Classcodes for RxRisk V based on ATC codes

Note that desired implementation might differ over time and by country.

## Usage

``` r
rxriskv
```

## Format

Data frames with 46 rows and 6 variables:

- group:

  medical condition

- pratt:

  ATC codes from table 1 in Pratt et al. 2018 (ignoring PBS item codes
  and extra conditions).

- garland:

  Modified version by Anne Garland to resemble medical use in Sweden
  2016 (Unpublished).

- caughey:

  From appendix 1 in Caughey et al. 2010

- pratt:

  Mortality weights from table 1 in Pratt et al. 2018

- sum_all:

  Unweighted count of all conditions.

## References

Caughey GE, Roughead EE, Vitry AI, McDermott RA, Shakib S, Gilbert AL.
Comorbidity in the elderly with diabetes: Identification of areas of
potential treatment conflicts. Diabetes Res Clin Pract 2010;87:385–93.

Pratt NL, Kerr M, Barratt JD, Kemp-Casey A, Kalisch Ellett LM, Ramsay E,
et al. The validity of the Rx-Risk Comorbidity Index using medicines
mapped to the Anatomical Therapeutic Chemical (ATC) Classification
System. BMJ Open 2018;8.

## See also

Other default classcodes:
[`ae`](https://docs.ropensci.org/coder/reference/ae.md),
[`charlson`](https://docs.ropensci.org/coder/reference/charlson.md),
[`cps`](https://docs.ropensci.org/coder/reference/cps.md),
[`elixhauser`](https://docs.ropensci.org/coder/reference/elixhauser.md),
[`hip_ae_hailer`](https://docs.ropensci.org/coder/reference/hip_ae_hailer.md)
