# Estimate Predictive Modeling-Based MICs and thresholds

Estimates (i) predictive modeling-based, (ii) adjusted predictive
modeling-based, and (iii) improved adjusted predictive modeling-based
MICs, with optional bootstrap confidence intervals. `mic_iapm` can also
be used to estimate the interpretation threshold of a predictor.

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
  seed = NULL,
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

- seed:

  Optional integer seed for reproducible bootstrap confidence intervals.

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

  Anchor reliability used in the iAPM calculation.

- nboot:

  Requested number of bootstrap samples.

- n_successful_boot:

  Number of successful bootstrap samples.

## Details

Based on methods developed by Terluin et al. (2015), Terluin et al.
(2017), and Terluin et al. (2022).

If `nboot >= 100`, bootstrap confidence intervals are computed for the
predictive MIC and adjusted predictive MIC. If `anchor_reliability` is
supplied, a bootstrap confidence interval is also computed for the
improved adjusted predictive MIC.

## References

Terluin B, Eekhout I, Terwee CB, de Vet HCW. Minimal important change
(MIC) based on a predictive modeling approach was more precise than MIC
based on ROC analysis. J Clin Epidemiol. 2015;68(12):1388-1396.
doi:10.1016/j.jclinepi.2015.03.015

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
sim <- simdat(N = 300, seed = 123, add_change = TRUE)
dat <- sim$datw

mic_iapm(
  mypred = "change",
  anchor = "trat",
  mydata = dat,
  anchor_reliability = sim$truth$observed_rel_trt,
  nboot = 0
)
#> $mic_pm
#> [1] 2.306145
#> 
#> $mic_apm
#> [1] 2.462398
#> 
#> $mic_iapm
#> [1] 2.752017
#> 
#> $anchor_reliability
#> [1] 0.6437079
#> 
#> $mic_pm_ci
#> NULL
#> 
#> $mic_apm_ci
#> NULL
#> 
#> $mic_iapm_ci
#> NULL
#> 
#> $mic_ci
#> NULL
#> 
#> $nboot
#> [1] 0
#> 
#> $n_successful_boot
#> [1] 0
#> 
#> attr(,"class")
#> [1] "mic_iapm"
```
