# Predicted Minimal Important Change (MIC)

Minimal Important Change (MIC) obtained as predicted value from a
logistic regression model.

## Usage

``` r
mic_adjust(data = NULL, x, y, tr, reliability)
```

## Arguments

- data:

  data.frame that holds the scores at two time points and the transition
  rate in separate columns.

- x:

  vector with score at time point 1, or column name in the data if
  `!is.null(data)`

- y:

  vector with score at time point 1, or column name in the data if
  `!is.null(data)`

- tr:

  vector with transition rates (perceived change), or column name in the
  data if `!is.null(data)`

- reliability:

  the reliability for the transition score. Can be computed with the
  [`tr_reliability()`](https://yhpua.github.io/MiCT/reference/tr_reliability.md)
  function.

## Value

vector with the adjusted MIC value

## Details

`mic_adjust()` is a focused function for estimating the adjusted
predictive modeling-based MIC. For an all-in-one workflow that returns
the predictive MIC, adjusted predictive MIC, and improved adjusted
predictive MIC with optional bootstrap confidence intervals, see
[`mic_iapm()`](https://yhpua.github.io/MiCT/reference/mic_iapm.md).

## See also

[`mic_iapm()`](https://yhpua.github.io/MiCT/reference/mic_iapm.md),
[`tr_reliability()`](https://yhpua.github.io/MiCT/reference/tr_reliability.md)

## Examples

``` r
data(example)
nitems <- 10
example$x <- rowSums(example[,1:nitems])                 # sumscore T1
example$y <- rowSums(example[,(nitems+1):(2*nitems)])    # sumscore T2
mic_adjust(x = example$x, y = example$y, tr = example$trat, reliability = 0.5)
#> adjusted MIC 
#>     2.757705 
mic_adjust(data = example, x = "x", y = "y", tr = "trat", reliability = 0.5)
#> adjusted MIC 
#>     2.757705 


# For predictive, adjusted, and improved adjusted MICs in one workflow,
# see mic_iapm().
```
