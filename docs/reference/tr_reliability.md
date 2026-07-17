# Estimate anchor reliability using CFA

`tr_reliability()` estimates the reliability of an anchor or transition
rating item using confirmatory factor analysis. The reliability estimate
is the R-squared value of the anchor item from the fitted CFA model.

## Usage

``` r
tr_reliability(
  data,
  model = NULL,
  anchor = NULL,
  xsec = FALSE,
  pair_by = c("position", "suffix"),
  t1_suffix = NULL,
  t2_suffix = NULL,
  factor_names = NULL,
  item_type = NULL,
  continuous_items = NULL,
  modification = TRUE,
  mi_cut = 0.3,
  complete_cases = TRUE,
  print_model = TRUE,
  verbose = TRUE,
  ...
)
```

## Arguments

- data:

  A data frame containing items and an anchor variable.

- model:

  Optional lavaan model syntax. If `NULL`, syntax is generated
  automatically using
  [`tr_reliability_model()`](https://yhpua.github.io/MiCT/reference/tr_reliability_model.md).

- anchor:

  Character. Name of the anchor variable. If `NULL`, the final column of
  `data` is treated as the anchor.

- xsec:

  Logical. If `TRUE`, estimate cross-sectional anchor reliability. If
  `FALSE`, estimate longitudinal anchor / transition-rating reliability.

- pair_by:

  Pairing method for longitudinal data. `"position"` assumes Time 1
  items followed by Time 2 items. `"suffix"` detects pairs using
  `t1_suffix` and `t2_suffix`.

- t1_suffix:

  Regex suffix identifying Time 1 items when `pair_by = "suffix"`.

- t2_suffix:

  Regex suffix identifying Time 2 items when `pair_by = "suffix"`.

- factor_names:

  Optional character vector of factor name(s). If `NULL`, defaults to
  `"F1"` when `xsec = TRUE`, and `c("F1", "F2")` when `xsec = FALSE`.

- item_type:

  Optional character. Type of items used as indicators. If `NULL`,
  defaults to `"ordinal"` unless `continuous_items` is supplied, in
  which case it defaults to `"mixed"`. `"ordinal"` treats all items and
  the anchor as ordered categorical variables. `"continuous"` treats all
  items as continuous while still treating the anchor as ordered.
  `"mixed"` treats all items as ordered except those listed in
  `continuous_items`; the anchor is always treated as ordered.

- continuous_items:

  Optional character vector of item names to treat as continuous. If
  supplied and `item_type = NULL`, `item_type` is automatically set to
  `"mixed"`. Do not include the anchor variable; it is always treated as
  ordered.

- modification:

  Logical. If `TRUE`, return modification indices with
  `sepc.lv > mi_cut`.

- mi_cut:

  Numeric. Cutoff for standardized latent-variable modification indices.

- complete_cases:

  Logical. If `TRUE`, retain only complete cases before fitting the
  model.

- print_model:

  Logical. If `TRUE`, print the generated lavaan syntax.

- verbose:

  Logical. If `TRUE`, print progress messages.

- ...:

  Additional arguments passed to
  [`lavaan::cfa()`](https://rdrr.io/pkg/lavaan/man/cfa.html).

## Value

An object of class `tr_reliability`.

## Details

This function follows the CFA approach for estimating transition-rating
reliability described by Griffiths et al. (2022). For longitudinal data,
Time 1 items load on the first factor, Time 2 items load on the second
factor, and the anchor loads on both factors. Residuals of corresponding
items across time-points are allowed to correlate. No constraints are
placed on loadings or thresholds.

For cross-sectional data, a one-factor CFA model is used, with the
anchor item included as an indicator of the latent factor.

## References

Griffiths P, Terluin B, Trigg A, Schuller W, Bjorner JB. A confirmatory
factor analysis approach was found to accurately estimate the
reliability of transition ratings. J Clin Epidemiol. 2022;141:36-45.
doi:10.1016/j.jclinepi.2021.08.029

## See also

[`tr_reliability_model()`](https://yhpua.github.io/MiCT/reference/tr_reliability_model.md)

## Examples

``` r
if (FALSE) { # \dontrun{
sim <- simdat(N = 300, seed = 123)
dat <- sim$datw

rel <- tr_reliability(
  data = dat[, c(sim$item_names$t1_items,
                 sim$item_names$t2_items,
                 "trat")],
  anchor = "trat",
  xsec = FALSE,
  pair_by = "suffix",
  t1_suffix = "",
  t2_suffix = "\\.1",
  item_type = "ordinal",
  modification = FALSE,
  print_model = FALSE
)

rel
} # }
```
