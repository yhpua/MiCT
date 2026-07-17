# Estimate a CFA-based interpretation threshold for a continuous single-item measure

`sim_threshold()` estimates an interpretation threshold for a continuous
single-item measure using a confirmatory factor analysis (CFA) approach.

## Usage

``` r
sim_threshold(
  mydata,
  var_formula,
  sim_levels = 10L,
  ordered = NULL,
  add_lmodel = NULL,
  tr_cut = 1,
  B = 0L,
  report_every = 50L,
  require_all_sim_levels = FALSE,
  factor_name = "F1",
  sim_suffix = "_ord",
  std.lv = TRUE,
  parameterization = "theta",
  verbose = FALSE,
  ...
)
```

## Arguments

- mydata:

  Data frame.

- var_formula:

  Formula of the form `tr ~ sim + aux1 + aux2 + ...`. The left-hand side
  is the transition or anchor variable. The first right-hand side
  variable is the continuous SIM. Remaining right-hand side variables
  are auxiliary CFA indicators.

- sim_levels:

  Number of equal-width levels for discretizing the SIM. Must be between
  2 and 12.

- ordered:

  Additional auxiliary variables to treat as ordered in lavaan.

- add_lmodel:

  Optional lavaan syntax appended to the generated model.

- tr_cut:

  Cutpoint for binarizing the transition variable when it has more than
  two unique non-missing values. Values \>= `tr_cut` are coded 1. If the
  transition variable already has exactly two unique values, `tr_cut` is
  ignored.

- B:

  Number of bootstrap samples. Bootstrap CI is computed only if
  `B >= 100`.

- report_every:

  During bootstrapping, print progress every `report_every` attempted
  fits.

- require_all_sim_levels:

  Passed to
  [`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md).

- factor_name:

  Name of the latent factor.

- sim_suffix:

  Suffix appended to the SIM variable name after discretization.

- std.lv:

  Passed to [`lavaan::cfa()`](https://rdrr.io/pkg/lavaan/man/cfa.html).

- parameterization:

  Passed to [`lavaan::cfa()`](https://rdrr.io/pkg/lavaan/man/cfa.html).

- verbose:

  If `TRUE`, print generated lavaan model.

- ...:

  Additional arguments passed to
  [`lavaan::cfa()`](https://rdrr.io/pkg/lavaan/man/cfa.html).

## Value

A `sim_threshold` object. The printed output is compact. Additional
details can be retrieved with
[`sim_threshold_details()`](https://yhpua.github.io/MiCT/reference/sim_threshold_details.md).

## Details

The technique is based on:

Terluin, B., Pua, Y.H., Fromy, P. et al. Estimating the minimal
important change of single-item measures using the adjusted predictive
modeling method or the longitudinal confirmatory factor analysis method.
Qual Life Res 35, 39 (2026). DOI: 10.1007/s11136-025-04134-3

The `sim_threshold()` function:

1.  discretizes a continuous SIM into equal-width ordered categories;

2.  fits a one-factor CFA model with an anchor transition item;

3.  computes `theta_star`, the latent factor value where the anchor
    threshold occurs;

4.  maps `theta_star` back to the original SIM scale using CFA-implied
    category probabilities and bin midpoints.

## Examples

``` r
if (FALSE) { # \dontrun{
sim <- simdat(N = 500, seed = 123)
dat <- sim$datw
t1_items <- sim$item_names$t1_items

dat_t1 <- dat[, c(t1_items, "trat")]

dat_sim <- dat_t1
dat_sim$sim8 <- rowSums(dat_sim[, t1_items[1:8], drop = FALSE])
dat_sim$aux9 <- dat_sim[[t1_items[9]]]
dat_sim$aux10 <- dat_sim[[t1_items[10]]]
dat_sim <- dat_sim[, c("sim8", "aux9", "aux10", "trat")]

out <- sim_threshold(
  mydata = dat_sim,
  var_formula = trat ~ sim8 + aux9 + aux10,
  sim_levels = 10,
  ordered = c("aux9", "aux10"),
  B = 0
)

out
} # }

```
