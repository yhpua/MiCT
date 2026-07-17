# Estimate Present-State Bias and Anchor-Based MIC Using Longitudinal CFA

`mic_lcfa()` estimates present-state bias and anchor-based minimal
important change (MIC) using a longitudinal confirmatory factor analysis
(LCFA) model.

## Usage

``` r
mic_lcfa(
  mydat,
  trt = NULL,
  model = NULL,
  trt_cut = 1,
  auto_equalize = TRUE,
  pair_by = c("position", "suffix"),
  t1_suffix = NULL,
  t2_suffix = NULL,
  min_resp = 5L,
  B = 0L,
  report_every = 100L,
  N_ets = 5000L,
  score_method = c("irt", "cfa", "both"),
  return_prepared = FALSE,
  print_model = TRUE,
  verbose = TRUE
)
```

## Arguments

- mydat:

  Data frame containing paired Time 1 and Time 2 PROM items and one
  transition rating variable.

- trt:

  Character. Name of the transition rating variable. If `NULL`, the
  final column of `mydat` is used.

- model:

  Optional lavaan model syntax. If `NULL`, the model is generated
  automatically using
  [`lcfa_model()`](https://yhpua.github.io/MiCT/reference/lcfa_model.md).

- trt_cut:

  Numeric. Cutpoint used to dichotomize the transition rating variable
  when it has more than two observed values. Values greater than or
  equal to `trt_cut` are coded as 1, and lower values are coded as 0. If
  the transition rating is already binary, the larger value is coded as
  1 and `trt_cut` is ignored.

- auto_equalize:

  Logical. If `TRUE`, item levels are equalized using
  [`equalize_levels()`](https://yhpua.github.io/MiCT/reference/equalize_levels.md).

- pair_by:

  Character. Pairing method passed to
  [`equalize_levels()`](https://yhpua.github.io/MiCT/reference/equalize_levels.md)
  and
  [`lcfa_model()`](https://yhpua.github.io/MiCT/reference/lcfa_model.md).
  `"position"` assumes Time 1 items followed by Time 2 items. `"suffix"`
  detects pairs using `t1_suffix` and `t2_suffix`.

- t1_suffix:

  Regex suffix identifying Time 1 items when `pair_by = "suffix"`.

- t2_suffix:

  Regex suffix identifying Time 2 items when `pair_by = "suffix"`.

- min_resp:

  Integer. Minimum number of responses required in each response
  category before collapsing.

- B:

  Integer. Number of bootstrap samples. Bootstrap confidence intervals
  are computed only if `B >= 100`.

- report_every:

  Integer. Progress message interval during bootstrapping.

- N_ets:

  Integer. Number of simulated theta values used for the IRT expected
  test score calculation. This argument is not used by the closed-form
  CFA method.

- score_method:

  Character. Method used to map the latent MIC to the observed PROM
  score metric. Options are `"irt"`, `"cfa"`, and `"both"`. `"irt"` uses
  [`mirt::expected.test()`](https://philchalmers.github.io/mirt/reference/expected.test.html),
  `"cfa"` uses the closed-form CFA/probit expected-score method, and
  `"both"` returns both mappings.

- return_prepared:

  Logical. If `TRUE`, returns prepared data, generated model, fitted
  LCFA object, and bootstrap details.

- print_model:

  Logical. If `TRUE`, prints the generated lavaan model.

- verbose:

  Logical. If `TRUE`, prints progress messages.

## Value

A `mic_lcfa` object with elements including:

- `psb`: estimated present-state bias;

- `MIC.theta`: MIC on the latent-change scale;

- `MIC.ets`: MIC on the observed PROM summed-score metric;

- `MIC.ets.irt`: IRT-based expected-score MIC, if requested;

- `MIC.ets.cfa`: CFA-based expected-score MIC, if requested;

- `MIC_CI`: bootstrap confidence interval, if requested;

- `nboot`: number of successful bootstrap estimates.

If `return_prepared = TRUE`, the returned object also contains the
prepared data, generated lavaan model, fitted LCFA object, and bootstrap
values.

## Details

The input data should contain paired Time 1 and Time 2 PROM items plus
one transition rating variable. Item pairs may be identified by column
position or by suffixes. By default, paired item levels are first
equalized using
[`equalize_levels()`](https://yhpua.github.io/MiCT/reference/equalize_levels.md),
and lavaan syntax is generated using
[`lcfa_model()`](https://yhpua.github.io/MiCT/reference/lcfa_model.md).

The LCFA model estimates the MIC on the latent-change scale. This latent
MIC is then mapped onto the observed PROM summed-score metric using an
expected score function.

The LCFA model uses both Time 1 and Time 2 item responses, together with
the transition rating, to estimate the latent MIC and present-state
bias. The latent MIC is then mapped onto the observed PROM summed-score
metric.

1.  at the baseline latent trait distribution, `theta ~ N(0, 1)`;

2.  at the shifted latent trait distribution, `theta ~ N(MIC.theta, 1)`.

Two expected-score mappings are available through `score_method`:

- `"irt"` uses an IRT-based expected test score method via
  [`mirt::expected.test()`](https://philchalmers.github.io/mirt/reference/expected.test.html).
  This follows the published implementation of Terluin et al. (2024).

- `"cfa"` uses a closed-form CFA probit expected-score method based on
  lavaan-estimated item loadings and thresholds. For each item, the
  method computes marginal category probabilities under
  `theta ~ N(mu, 1)`, first with `mu = 0` and then with
  `mu = MIC.theta`. These probabilities are used to obtain expected item
  scores, which are summed across items to obtain the expected PROM
  score.

- `"both"` computes expected scores using both irt and cfa methods

Under the CFA probit method, the closed-form marginal cumulative
probability for item threshold `tau_k` is based on the latent response
model `Y* = lambda * theta + error`. If `theta ~ N(mu, 1)` and
`error ~ N(0, 1)`, then `Y* ~ N(lambda * mu, lambda^2 + 1)`. Therefore,
marginal category probabilities can be computed without simulating theta
values or fitting an additional IRT model.

## References

Terluin B, Trigg A, Fromy P, Schuller W, Terwee CB, Bjorner JB.
Estimating anchor-based minimal important change using longitudinal
confirmatory factor analysis. Qual Life Res. 2024;33:963-973.
doi:10.1007/s11136-023-03577-w

Terluin B, Fromy P, Trigg A, Terwee CB, Bjorner JB. Effect of present
state bias on minimal important change estimates: a simulation study.
Quality of Life Research. 2024. doi:10.1007/s11136-024-03763-4

Terluin B, Griffiths P, Trigg A, Terwee CB, Bjorner JB. Present state
bias in transition ratings was accurately estimated in simulated and
real data. J Clin Epidemiol. 2022;143:128-136.
doi:10.1016/j.jclinepi.2021.12.024

## See also

[`lcfa_model()`](https://yhpua.github.io/MiCT/reference/lcfa_model.md),
[`equalize_levels()`](https://yhpua.github.io/MiCT/reference/equalize_levels.md),
[`simdat()`](https://yhpua.github.io/MiCT/reference/simdat.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# Minimal working example
sim <- simdat(N = 500, seed = 123, add_change = TRUE)
dat <- sim$datw

mydat <- dat[, c(
  sim$item_names$t1_items,
  sim$item_names$t2_items,
  "trat"
)]

# Compare IRT and CFA mappings
out_both <- mic_lcfa(
  mydat = mydat,
  trt = "trat",
  trt_cut = 1,
  auto_equalize = TRUE,
  pair_by = "suffix",
  t1_suffix = "",
  t2_suffix = "\\.1",
  min_resp = 5,
  score_method = "both",
  B = 0,
  print_model = FALSE,
  verbose = FALSE)

out_both$MIC.ets

# Bootstrap confidence interval using the faster CFA mapping
out_boot <- mic_lcfa(
  mydat = mydat,
  trt = "trat",
  trt_cut = 1,
  auto_equalize = TRUE,
  pair_by = "suffix",
  t1_suffix = "",
  t2_suffix = "\\.1",
  min_resp = 5,
  score_method = "cfa",
  B = 200,
  report_every = 50,
  print_model = FALSE,
  verbose = TRUE)

out_boot
} # }
```
