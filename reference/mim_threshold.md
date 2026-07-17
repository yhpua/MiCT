# Estimate a CFA-based threshold for a multi-item questionnaire

`mim_threshold()` estimates an anchor-based interpretation threshold for
a multi-item measure (MIM) using a one-factor CFA model with an
anchor/transition rating item.

## Usage

``` r
mim_threshold(
  mydata,
  var_formula,
  item_discretize = FALSE,
  item_levels = 10L,
  require_all_item_levels = FALSE,
  anchor_cut = 1,
  B = 0L,
  report_every = 50L,
  factor_name = "F1",
  item_suffix = "_ord",
  anchor_suffix = "_bin",
  std.lv = TRUE,
  parameterization = "theta",
  verbose = TRUE,
  ...
)
```

## Arguments

- mydata:

  Data frame.

- var_formula:

  Formula of the form `anchor ~ item1 + item2 + ...`. Use `anchor ~ .`
  to use all variables except the anchor as items.

- item_discretize:

  Logical. If `TRUE`, discretize each item using
  [`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md).
  If `FALSE`, use the item values as observed ordinal categories.

- item_levels:

  Number of levels to use when `item_discretize = TRUE`. Must be between
  2 and 12.

- require_all_item_levels:

  Logical. Passed to
  [`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md).

- anchor_cut:

  Cutpoint for binarizing the anchor if it has more than two unique
  non-missing values. Values \>= `anchor_cut` are coded 1.

- B:

  Number of bootstrap samples. Bootstrap CI is computed only if
  `B >= 100`.

- report_every:

  During bootstrapping, print progress every `report_every` attempted
  fits.

- factor_name:

  Name of the latent factor.

- item_suffix:

  Suffix appended to item names when items are discretized or internally
  recoded.

- anchor_suffix:

  Suffix appended to the anchor variable name if the anchor has more
  than two unique values and is dichotomized.

- std.lv:

  Passed to [`lavaan::cfa()`](https://rdrr.io/pkg/lavaan/man/cfa.html).

- parameterization:

  Passed to [`lavaan::cfa()`](https://rdrr.io/pkg/lavaan/man/cfa.html).

- verbose:

  If `TRUE`, print item names and generated lavaan model.

- ...:

  Additional arguments passed to
  [`lavaan::cfa()`](https://rdrr.io/pkg/lavaan/man/cfa.html).

## Value

A `mim_threshold` object. Additional details can be retrieved with
[`mim_threshold_details()`](https://yhpua.github.io/MiCT/reference/mim_threshold_details.md).

## Details

This technique is based on Terluin et al (2023) and Terluin et al
(2024). Briefly, the `mim_threshold()` function:

1.  fits a one-factor CFA model with the MIM items and an anchor item,

2.  computes `theta_star`, the latent factor value where the anchor,
    threshold occurs,

3.  maps `theta_star` to each item using CFA-implied category
    probabilities,

4.  multiplies item category probabilities by item values or bin
    midpoints, and

5.  sums the item-level expected values to obtain the MIM threshold.

## References

Terluin, B., Koopman, J.E., Hoogendam, L. et al. Estimating meaningful
thresholds for multi-item questionnaires using item response theory.
Qual Life Res 32, 1819–1830 (2023)

Terluin, B., Trigg, A., Fromy, P. et al. Estimating anchor-based minimal
important change using longitudinal confirmatory factor analysis. Qual
Life Res 33, 963–973 (2024).

## Examples

``` r
if (FALSE) { # \dontrun{
sim <- simdat(N = 500, seed = 123)
dat <- sim$datw
t1_items <- sim$item_names$t1_items

dat_t1 <- dat[, c(t1_items, "trat")]

out <- mim_threshold(
  mydata = dat_t1,
  var_formula = trat ~ .,
  B = 0
)

out
} # }
```
