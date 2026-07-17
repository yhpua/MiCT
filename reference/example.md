# Simulated longitudinal MIC example dataset

A simulated dataset containing responses to a 10-item patient-reported
outcome measure at two time points for 1000 subjects, together with a
binary transition rating anchor.

## Usage

``` r
example
```

## Format

A data frame with 1000 rows and 21 variables:

- v1_1:

  Item 1 at Time 1.

- v1_2:

  Item 2 at Time 1.

- v1_3:

  Item 3 at Time 1.

- v1_4:

  Item 4 at Time 1.

- v1_5:

  Item 5 at Time 1.

- v1_6:

  Item 6 at Time 1.

- v1_7:

  Item 7 at Time 1.

- v1_8:

  Item 8 at Time 1.

- v1_9:

  Item 9 at Time 1.

- v1_10:

  Item 10 at Time 1.

- v2_1:

  Item 1 at Time 2.

- v2_2:

  Item 2 at Time 2.

- v2_3:

  Item 3 at Time 2.

- v2_4:

  Item 4 at Time 2.

- v2_5:

  Item 5 at Time 2.

- v2_6:

  Item 6 at Time 2.

- v2_7:

  Item 7 at Time 2.

- v2_8:

  Item 8 at Time 2.

- v2_9:

  Item 9 at Time 2.

- v2_10:

  Item 10 at Time 2.

- trat:

  Binary transition rating anchor, coded 0/1.

## Source

Simulated example data generated for demonstrating MIC package
functions. See `data-raw/R/example.R`.

## Details

Each item has four ordered response categories scored 0, 1, 2, and 3.
Therefore, the summed score at each time point ranges from 0 to 30.

The Time 1 items are named `v1_1` to `v1_10`, and the Time 2 items are
named `v2_1` to `v2_10`. The variable `trat` is a dichotomous transition
rating indicator coded 0/1.

This dataset is useful for demonstrating MIC estimation functions such
as [`mic_roc()`](https://yhpua.github.io/MiCT/reference/mic_roc.md),
[`mic_pred()`](https://yhpua.github.io/MiCT/reference/mic_pred.md),
[`mic_adjust()`](https://yhpua.github.io/MiCT/reference/mic_adjust.md),
[`mic_iapm()`](https://yhpua.github.io/MiCT/reference/mic_iapm.md),
[`tr_reliability()`](https://yhpua.github.io/MiCT/reference/tr_reliability.md),
and [`mic_lcfa()`](https://yhpua.github.io/MiCT/reference/mic_lcfa.md).

## See also

[`simdat()`](https://yhpua.github.io/MiCT/reference/simdat.md) for
generating new simulated datasets with user-specified simulation
parameters.

[`mic_roc()`](https://yhpua.github.io/MiCT/reference/mic_roc.md),
[`mic_pred()`](https://yhpua.github.io/MiCT/reference/mic_pred.md),
[`mic_adjust()`](https://yhpua.github.io/MiCT/reference/mic_adjust.md),
and [`mic_iapm()`](https://yhpua.github.io/MiCT/reference/mic_iapm.md)
for predictive modeling and adjusted predictive modeling MIC estimation.

[`tr_reliability()`](https://yhpua.github.io/MiCT/reference/tr_reliability.md)
for estimating transition rating reliability.

[`mic_lcfa()`](https://yhpua.github.io/MiCT/reference/mic_lcfa.md) for
LCFA-based MIC estimation.

## Examples

``` r
data(example)

nitems <- 10
example$score_t1 <- rowSums(example[, paste0("v1_", 1:nitems)])
example$score_t2 <- rowSums(example[, paste0("v2_", 1:nitems)])
example$change <- example$score_t2 - example$score_t1

head(example)
#>   v1_1 v1_2 v1_3 v1_4 v1_5 v1_6 v1_7 v1_8 v1_9 v1_10 v2_1 v2_2 v2_3 v2_4 v2_5
#> 1    1    0    1    0    0    0    0    0    0     2    1    3    1    2    0
#> 2    3    2    0    2    2    0    1    1    3     0    2    0    2    2    2
#> 3    3    0    0    3    0    3    0    0    2     0    3    3    3    3    3
#> 4    2    0    0    0    2    1    0    0    0     3    0    3    1    2    2
#> 5    3    2    2    1    0    3    1    1    0     1    3    1    3    3    3
#> 6    3    3    2    0    0    0    0    3    0     0    3    0    3    0    0
#>   v2_6 v2_7 v2_8 v2_9 v2_10 trat score_t1 score_t2 change
#> 1    0    0    2    0     0    1        4        9      5
#> 2    2    2    1    0     0    0       14       13     -1
#> 3    3    3    2    2     2    1       11       27     16
#> 4    1    2    3    0     1    0        8       15      7
#> 5    0    2    3    2     3    1       14       23      9
#> 6    1    3    0    0     0    0       11       10     -1

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
```
