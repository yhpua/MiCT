# ROC-Based Minimal Important Change

Estimates the minimal important change (MIC) using receiver operating
characteristic (ROC) analysis. The MIC is estimated as the change-score
threshold that optimizes the Youden criterion.

## Usage

``` r
mic_roc(
  data = NULL,
  x,
  y,
  tr,
  nboot = 0,
  report_every = 100,
  verbose = FALSE,
  seed = NULL,
  max_attempts = nboot * 5
)
```

## Arguments

- data:

  Optional data frame containing the Time 1 score, Time 2 score, and
  binary anchor.

- x:

  Numeric vector of scores at Time 1, or character name of the Time 1
  score column in `data`.

- y:

  Numeric vector of scores at Time 2, or character name of the Time 2
  score column in `data`.

- tr:

  Binary anchor vector, or character name of the anchor column in
  `data`. The anchor must be coded as 0/1 or TRUE/FALSE.

- nboot:

  Integer. Number of bootstrap samples for estimating the 95% confidence
  interval. Bootstrapping is performed only when `nboot >= 100`.

- report_every:

  Integer. The interval at which bootstrap progress should be printed.

- verbose:

  Logical. If `TRUE`, progress messages are printed.

- seed:

  Optional integer seed for reproducible bootstrap confidence intervals.

- max_attempts:

  Integer. Maximum number of bootstrap attempts. This avoids an infinite
  loop when many bootstrap samples fail.

## Value

A `mic_roc` object containing the ROC-based MIC and, optionally,
bootstrap confidence intervals.

## Details

Optional bootstrap confidence intervals can be requested by setting
`nboot >= 100`.

## Examples

``` r
data(example)

nitems <- 10

example$score_t1 <- rowSums(example[, paste0("v1_", seq_len(nitems))])
example$score_t2 <- rowSums(example[, paste0("v2_", seq_len(nitems))])

mic_roc(
  data = example,
  x = "score_t1",
  y = "score_t2",
  tr = "trat",
  nboot = 0
)
#> Warning: 'transpose=TRUE' is deprecated. Only 'transpose=FALSE' will be allowed in a future version.
#> ROC-based MIC estimation
#> ------------------------
#> MIC ROC: -0.500 

if (FALSE) { # \dontrun{
mic_roc(
  data = example,
  x = "score_t1",
  y = "score_t2",
  tr = "trat",
  nboot = 500,
  seed = 123
)
} # }
```
