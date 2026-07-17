# Predicted Minimal Important Change (MIC)

Minimal Important Change (MIC) obtained as predicted value from a
logistic regression model.

## Usage

``` r
mic_pred(data = NULL, x, y, tr)
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

## Value

vector with the predicted MIC value

## Details

`mic_pred()` is a focused function for estimating the predictive
modeling-based MIC. For an all-in-one workflow that returns the
predictive MIC, adjusted predictive MIC, and improved adjusted
predictive MIC with optional bootstrap confidence intervals, see
[`mic_iapm()`](https://yhpua.github.io/MiCT/reference/mic_iapm.md).

## See also

[`mic_iapm()`](https://yhpua.github.io/MiCT/reference/mic_iapm.md)

## Examples

``` r

data(example)
nitems <- 10
example$x <- rowSums(example[,1:nitems])                 # sumscore T1
example$y <- rowSums(example[,(nitems+1):(2*nitems)])    # sumscore T2
mic_pred(x = example$x, y = example$y, tr = example$trat)
#> predicted MIC 
#>     0.8484085 
mic_pred(data = example, x = example$x, y = example$y, tr = example$trat)
#> Warning: data object is not used; separate x, y and tr input is used to compute the MIC.
#> predicted MIC 
#>     0.8484085 
mic_pred(data = example, x = "x", y = "y", tr = "trat")
#> predicted MIC 
#>     0.8484085 

# For predictive, adjusted, and improved adjusted MICs in one workflow,
# see mic_iapm().
```
