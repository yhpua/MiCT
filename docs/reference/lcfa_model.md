# Generate lavaan syntax for longitudinal CFA MIC estimation

`lcfa_model()` generates lavaan model syntax for estimating anchor-based
minimal important change (MIC) using longitudinal confirmatory factor
analysis.

## Usage

``` r
lcfa_model(
  data,
  trt,
  pair_by = c("position", "suffix"),
  t1_suffix = NULL,
  t2_suffix = NULL,
  pair_map = NULL,
  factor_t1 = "F1",
  factor_t2 = "F2",
  loading_prefix = "a",
  threshold_prefix = "b",
  trt_loading_t1 = "f1",
  trt_loading_t2 = "f2",
  trt_threshold_label = "thr.trt",
  psb_label = "psb",
  mic_label = "b_param",
  correlated_errors = TRUE,
  threshold_invariance = TRUE,
  include_residual_variances_t2 = TRUE,
  include_factor_structure = TRUE,
  include_comments = TRUE,
  print_model = TRUE
)
```

## Arguments

- data:

  A data frame containing paired Time 1 and Time 2 PROM items and a
  transition rating variable.

- trt:

  Character. Name of the transition rating variable.

- pair_by:

  Pairing method. `"position"` assumes the first half of item columns
  are Time 1 items and the second half are Time 2 items. `"suffix"`
  detects item pairs using `t1_suffix` and `t2_suffix`.

- t1_suffix:

  regex suffix identifying Time 1 items when `pair_by = "suffix"`. Use
  `""` when Time 1 items have no suffix.

- t2_suffix:

  regex suffix identifying Time 2 items when `pair_by = "suffix"`.

- pair_map:

  Optional data frame describing item pairs, typically from
  `equalize_levels()$pair_map`. If supplied, it takes precedence over
  `pair_by`, `t1_suffix`, and `t2_suffix`.

- factor_t1:

  Character. Name of the Time 1 latent factor.

- factor_t2:

  Character. Name of the Time 2 latent factor.

- loading_prefix:

  Character. Prefix for equality-constrained item loading labels.

- threshold_prefix:

  Character. Prefix for equality-constrained item threshold labels.

- trt_loading_t1:

  Character. Label for the transition rating loading on the Time 1
  factor.

- trt_loading_t2:

  Character. Label for the transition rating loading on the Time 2
  factor.

- trt_threshold_label:

  Character. Label for the transition rating threshold.

- psb_label:

  Character. Name of the defined present-state-bias parameter.

- mic_label:

  Character. Name of the defined MIC parameter on the latent theta
  scale.

- correlated_errors:

  Logical. If `TRUE`, add correlated residuals between corresponding
  Time 1 and Time 2 items.

- threshold_invariance:

  Logical. If `TRUE`, constrain thresholds equal across Time 1 and Time
  2 within each item pair.

- include_residual_variances_t2:

  Logical. If `TRUE`, frees residual variances of Time 2 items using
  `item_t2 ~~ NA*item_t2`.

- include_factor_structure:

  Logical. If `TRUE`, adds factor variances, covariance, and latent
  means/intercepts sections.

- include_comments:

  Logical. If `TRUE`, include section comments in the generated lavaan
  syntax.

- print_model:

  Logical. If `TRUE`, prints the generated lavaan syntax for easy
  inspection.

## Value

An object of class `lcfa_model`, invisibly. The generated lavaan syntax
can be accessed using `$model`.

## Details

The generated model is intended for ordinal PROM items and a binary
transition rating item. Item thresholds are constrained equal across
Time 1 and Time 2 within each item pair. The number of thresholds is
determined automatically from the observed response levels in the
supplied data.

Item pairs can be detected either by column position or by suffix
patterns, using the same logic as
[`equalize_levels()`](https://yhpua.github.io/MiCT/reference/equalize_levels.md).

The model defines:


    psb := (f1/f2) + 1
    b_param := thr.trt/f2

## Examples

``` r
sim <- simdat(N = 100, seed = 123)
dat <- sim$datw

mod <- lcfa_model(
  data = dat[, c(sim$item_names$t1_items,
                 sim$item_names$t2_items,
                 "trat")],
  trt = "trat",
  pair_by = "suffix",
  t1_suffix = "",
  t2_suffix = "\\.1",
  print_model = FALSE
)

mod$model
#> [1] "\n# Factors\nF1 =~ a1*item1 + a2*item2 + a3*item3 + a4*item4 + a5*item5 + a6*item6 + a7*item7 + a8*item8 + a9*item9 + a10*item10 + f1*trat\nF2 =~ a1*item1.1 + a2*item2.1 + a3*item3.1 + a4*item4.1 + a5*item5.1 + a6*item6.1 + a7*item7.1 + a8*item8.1 + a9*item9.1 + a10*item10.1 + f2*trat\n\n# Correlated errors over time\nitem1 ~~ item1.1\nitem2 ~~ item2.1\nitem3 ~~ item3.1\nitem4 ~~ item4.1\nitem5 ~~ item5.1\nitem6 ~~ item6.1\nitem7 ~~ item7.1\nitem8 ~~ item8.1\nitem9 ~~ item9.1\nitem10 ~~ item10.1\n\n# Thresholds\nitem1 + item1.1 | b1_1*t1 + b1_2*t2 + b1_3*t3\nitem2 + item2.1 | b2_1*t1 + b2_2*t2 + b2_3*t3\nitem3 + item3.1 | b3_1*t1 + b3_2*t2 + b3_3*t3\nitem4 + item4.1 | b4_1*t1 + b4_2*t2 + b4_3*t3\nitem5 + item5.1 | b5_1*t1 + b5_2*t2 + b5_3*t3\nitem6 + item6.1 | b6_1*t1 + b6_2*t2 + b6_3*t3\nitem7 + item7.1 | b7_1*t1 + b7_2*t2 + b7_3*t3\nitem8 + item8.1 | b8_1*t1 + b8_2*t2 + b8_3*t3\nitem9 + item9.1 | b9_1*t1 + b9_2*t2 + b9_3*t3\nitem10 + item10.1 | b10_1*t1 + b10_2*t2 + b10_3*t3\ntrat | thr.trt*t1\n\n# Variances/covariances\nF1 ~~ 1*F1\nF2 ~~ NA*F2\nF1 ~~ NA*F2\nitem1.1 ~~ NA*item1.1\nitem2.1 ~~ NA*item2.1\nitem3.1 ~~ NA*item3.1\nitem4.1 ~~ NA*item4.1\nitem5.1 ~~ NA*item5.1\nitem6.1 ~~ NA*item6.1\nitem7.1 ~~ NA*item7.1\nitem8.1 ~~ NA*item8.1\nitem9.1 ~~ NA*item9.1\nitem10.1 ~~ NA*item10.1\n\n# Means/intercepts\nF1 ~ 0*1\nF2 ~ NA*1\n\n# Derived parameters\npsb := (f1/f2) + 1\nb_param := thr.trt/f2\n"
```
