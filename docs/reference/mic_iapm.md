# Estimate Predictive Modeling-Based MICs and Thresholds

Estimates (i) predictive modeling-based, (ii) adjusted predictive
modeling-based, and (iii) improved adjusted predictive modeling-based
minimal important change (MIC) estimates, with optional bootstrap
confidence intervals. `mic_iapm()` can also be used to estimate the
interpretation threshold of a predictor.

## Usage

``` r
mic_iapm(
  mypred,
  anchor,
  mydata,
  anchor_reliability = NULL,
  nboot = 0,
  report_every = 100,
  verbose = FALSE,
  max_attempts = nboot * 5
)
```

## Arguments

- mypred:

  Character string; name of the column containing the change score or
  predictor score.

- anchor:

  Character string; name of the column containing the binary anchor. The
  anchor must be binary and coded as 0/1 or TRUE/FALSE.

- mydata:

  Data frame with the change score or predictor score and the anchor in
  separate columns.

- anchor_reliability:

  Optional anchor reliability. Can be either a single numeric value
  between 0 and 1, or an object returned by
  [`tr_reliability()`](https://yhpua.github.io/MiCT/reference/tr_reliability.md).
  If supplied, the improved adjusted predictive modeling-based MIC is
  also calculated.

- nboot:

  Integer; number of bootstrap samples for estimating 95% confidence
  intervals. Bootstrapping is performed only when `nboot >= 100`.

- report_every:

  Integer. The interval at which bootstrap progress should be printed.

- verbose:

  Logical. If `TRUE`, progress messages are printed.

- max_attempts:

  Integer; maximum number of bootstrap attempts. This avoids an infinite
  loop when many bootstrap samples fail.

## Value

A `mic_iapm` object containing:

- mic_pm:

  Predictive modeling-based MIC.

- mic_apm:

  Adjusted predictive modeling-based MIC.

- mic_iapm:

  Improved adjusted predictive modeling-based MIC, returned only when
  `anchor_reliability` is supplied.

- mic_pm_ci:

  Bootstrap confidence interval for `mic_pm`, if requested.

- mic_apm_ci:

  Bootstrap confidence interval for `mic_apm`, if requested.

- mic_iapm_ci:

  Bootstrap confidence interval for `mic_iapm`, if requested and
  `anchor_reliability` is supplied.

- mic_ci:

  Matrix of available MIC estimates and confidence intervals.

- anchor_reliability:

  Anchor reliability used in the improved adjusted predictive modeling
  calculation.

- nboot:

  Requested number of bootstrap samples.

- n_successful_boot:

  Number of successful bootstrap samples.

## Details

For reproducible bootstrap confidence intervals, call
[`set.seed()`](https://rdrr.io/r/base/Random.html) before calling
`mic_iapm()`.

## References

Terluin B, Eekhout I, Terwee CB, de Vet HCW. Minimal important change
(MIC) based on a predictive modeling approach was more precise than MIC
based on receiver operating characteristic analysis. J Clin Epidemiol.
2015;68(12):1388-1396. doi:10.1016/j.jclinepi.2015.03.015

Terluin B, Eekhout I, Terwee CB. The anchor-based minimal important
change, based on receiver operating characteristic analysis or
predictive modeling, may need to be adjusted for the proportion of
improved patients. J Clin Epidemiol. 2017;83:90-100.
doi:10.1016/j.jclinepi.2016.12.015

Terluin B, Eekhout I, Terwee CB. Improved adjusted minimal important
change took reliability of transition ratings into account. J Clin
Epidemiol. 2022;148:48-53. doi:10.1016/j.jclinepi.2022.04.018

## See also

[`tr_reliability()`](https://yhpua.github.io/MiCT/reference/tr_reliability.md)

## Examples

``` r
# \donttest{
set.seed(123)
sim <- simdat(N = 200, add_change = TRUE)
dat <- sim$datw

mic_iapm(
  mypred = "change",
  anchor = "trat",
  mydata = dat,
  anchor_reliability = sim$truth$observed_rel_trt,
  nboot = 200
)
#> $mic_pm
#> [1] 1.797553
#> 
#> $mic_apm
#> [1] 1.892168
#> 
#> $mic_iapm
#> [1] 1.994625
#> 
#> $anchor_reliability
#> [1] 0.7617496
#> 
#> $mic_pm_ci
#>      mic    lower    upper 
#> 1.797553 1.015572 2.725703 
#> 
#> $mic_apm_ci
#>      mic    lower    upper 
#> 1.892168 1.119443 2.799902 
#> 
#> $mic_iapm_ci
#>      mic    lower    upper 
#> 1.994625 0.982196 3.114250 
#> 
#> $mic_ci
#>               mic    lower    upper
#> mic_pm   1.797553 1.015572 2.725703
#> mic_apm  1.892168 1.119443 2.799902
#> mic_iapm 1.994625 0.982196 3.114250
#> 
#> $nboot
#> [1] 200
#> 
#> $n_successful_boot
#> [1] 200
#> 
#> attr(,"class")
#> [1] "mic_iapm"
# }
```
