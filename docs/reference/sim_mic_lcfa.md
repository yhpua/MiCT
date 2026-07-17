# LCFA-Based MIC for a Single-Item Measure

`sim_mic_lcfa()` estimates the minimal important change (MIC) for a
single-item measure (SIM) using longitudinal confirmatory factor
analysis with an auxiliary variable. The SIM is measured at Time 1 and
Time 2, an auxiliary variable is measured at Time 1 and Time 2, and a
transition rating is included as an anchor.

## Usage

``` r
sim_mic_lcfa(
  mydata,
  sim,
  aux,
  trt,
  trt_cut = 1,
  sim_levels = 10L,
  sim_discretize = c("auto", "yes", "no"),
  min_resp = 1L,
  aux_ordered = FALSE,
  B = 0L,
  report_every = 50L,
  seed = NULL,
  add_lmodel = NULL,
  print_model = FALSE,
  verbose = FALSE,
  ...
)
```

## Arguments

- mydata:

  Data frame.

- sim:

  Character vector of length 2. Names of the SIM variable at Time 1 and
  Time 2.

- aux:

  Character vector of length 2. Names of the auxiliary variable at Time
  1 and Time 2.

- trt:

  Character. Name of the transition rating / anchor variable.

- trt_cut:

  Numeric. Cutpoint used to binarize `trt` if it has more than two
  observed values. Values greater than or equal to `trt_cut` are
  coded 1. If `trt` already has exactly two observed values, the larger
  value is coded as 1 and `trt_cut` is ignored.

- sim_levels:

  Integer. Number of ordered levels used when discretizing the SIM with
  [`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md).
  Must be between 2 and 12. Default is 10.

- sim_discretize:

  Character. One of `"auto"`, `"yes"`, or `"no"`. `"auto"` applies
  [`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md)
  only when the pooled SIM has more than 12 observed levels. `"yes"`
  always applies
  [`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md).
  `"no"` never discretizes and errors if the pooled SIM has more than 12
  observed levels.

- min_resp:

  Integer. Minimum response count per category used by
  [`equalize_levels()`](https://yhpua.github.io/MiCT/reference/equalize_levels.md)
  when equalizing SIM response levels across time.

- aux_ordered:

  Logical. If `TRUE`, treats the auxiliary variables as ordered
  indicators in lavaan. Auxiliary variable loadings and thresholds are
  freely estimated over time.

- B:

  Integer. Number of nonparametric bootstrap samples. Bootstrap
  confidence intervals are computed only when `B >= 100`.

- report_every:

  Integer. Print bootstrap progress every `report_every` attempted
  bootstrap fits.

- seed:

  Optional integer seed for reproducibility.

- add_lmodel:

  Optional lavaan syntax appended to the generated model.

- print_model:

  Logical. If `TRUE`, prints the generated lavaan model.

- verbose:

  Logical. If `TRUE`, prints progress messages.

- ...:

  Additional arguments passed to
  [`lavaan::cfa()`](https://rdrr.io/pkg/lavaan/man/cfa.html).

## Value

A `sim_mic_lcfa` object with elements including:

- `MIC.theta`: MIC on the latent-change scale;

- `MIC.sim.transformed`: MIC on the transformed SIM scale;

- `MIC.sim`: MIC on the original SIM metric;

- `rel_SIM1`: model R-squared / reliability of SIM at Time 1;

- `rel_trt`: model R-squared / reliability of the transition rating;

- `psb`: estimated present-state bias;

- `ci`: bootstrap confidence intervals, if requested;

- `sim_prepared`: information about discretization, equalization, and
  back-transformation.

## Details

If the pooled SIM values across Time 1 and Time 2 have 12 or fewer
observed levels, the original SIM values are used as ordered categories.
If the pooled SIM values have more than 12 observed levels, the SIM is
discretized using
[`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md)
with common equal-width bins across Time 1 and Time 2. In both cases,
SIM response levels are equalized across time before fitting the LCFA
model.

The SIM loading and SIM thresholds are constrained equal over time so
that the Time 1 and Time 2 latent factors are on the same measurement
scale.

The latent MIC is estimated as `MIC.theta = tau_trt / f2`, where
`tau_trt` is the transition-rating threshold and `f2` is the
transition-rating loading on the Time 2 factor.

The SIM-scale MIC is first calculated on the transformed SIM scale as
`sqrt(var(SIM_T1_transformed) * Rel_SIM_T1) * MIC.theta`, where
`Rel_SIM_T1` is the model R-squared of the Time 1 SIM. If
[`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md)
is used, the transformed-scale MIC is then back-transformed to the
original SIM metric using the equal-width bin width.

Continuous SIMs with many distinct values are not used directly as
ordered indicators. When discretization is needed, `sim_mic_lcfa()`
applies
[`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md)
to the pooled Time 1 and Time 2 SIM values. This ensures that the same
equal-width binning rule is applied at both time points.

The LCFA model is fitted to the discretized/equalized ordered SIM
variables. Therefore, the reliability of the SIM, `Rel_SIM_T1`, is the
reliability of the transformed SIM used in the CFA model. For this
reason, the SIM MIC is first computed on the transformed SIM scale:

`MIC_SIM_transformed = sqrt(var(SIM_T1_transformed) * Rel_SIM_T1) * MIC.theta`.

If discretization was used, this transformed-scale MIC is then converted
back to the original SIM metric using:

`MIC_SIM_original = MIC_SIM_transformed * backtransform_factor`,

where `backtransform_factor` is the equal-width bin width, calculated as
the pooled original SIM range divided by the number of requested
discretized levels. If no discretization is used,
`backtransform_factor = 1`.

## References

Terluin B, Pua YH, Fromy P, Trigg A, van der Zwaard B, Bjorner JB.
Estimating the minimal important change of single-item measures using
the adjusted predictive modeling method or the longitudinal confirmatory
factor analysis method. Quality of Life Research. 2026.
doi:10.1007/s11136-025-04134-3

Terluin B, Trigg A, Fromy P, Schuller W, Terwee CB, Bjorner JB.
Estimating anchor-based minimal important change using longitudinal
confirmatory factor analysis. Qual Life Res. 2024;33:963-973.
doi:10.1007/s11136-023-03577-w

## See also

[`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md),
[`equalize_levels()`](https://yhpua.github.io/MiCT/reference/equalize_levels.md),
[`mic_lcfa()`](https://yhpua.github.io/MiCT/reference/mic_lcfa.md),
[`simdat()`](https://yhpua.github.io/MiCT/reference/simdat.md)

## Examples

``` r
if (FALSE) { # \dontrun{
sim <- simdat(N = 500, seed = 123, add_change = TRUE)
dat <- sim$datw

# Create a toy single-item measure from several items
dat$sim_t1 <- rowSums(dat[, paste0("item", 1:8), drop = FALSE])
dat$sim_t2 <- rowSums(dat[, paste0("item", 1:8, ".1"), drop = FALSE])

out <- sim_mic_lcfa(
  mydata = dat,
  sim = c("sim_t1", "sim_t2"),
  aux = c("item9", "item9.1"),
  trt = "trat",
  sim_discretize = "auto",
  sim_levels = 10,
  aux_ordered = TRUE,
  B = 0,
  print_model = TRUE
)

out

# Inspect how the SIM was prepared
out$sim_prepared$used_discretization
out$sim_prepared$backtransform_factor
out$sim_prepared$original_levels
} # }
```
