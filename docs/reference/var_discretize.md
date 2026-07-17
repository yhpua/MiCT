# Discretize a continuous variable into equal-width ordered categories

`var_discretize()` converts a continuous numeric vector into equal-width
integer categories. It is intended for creating ordered indicators for
lavaan-based CFA workflows. To be used with `sim_threshold_cfa`

## Usage

``` r
var_discretize(
  x,
  n_levels = 10L,
  min_level = 1L,
  require_all_levels = FALSE,
  warn_unused = TRUE
)
```

## Arguments

- x:

  Numeric vector to discretize.

- n_levels:

  Integer. Number of equal-width categories to create. Must be between 2
  and 12.

- min_level:

  Integer. Lowest score value. Defaults to `1L`.

- require_all_levels:

  Logical. If `TRUE`, stop when the discretized variable does not use
  all requested levels.

- warn_unused:

  Logical. If `TRUE`, warn when some levels are unused. Ignored when
  `require_all_levels = TRUE`.

## Value

A list containing the discretized score, bin midpoints, bin edges, used
levels, and unused levels.

## Details

Missing values are allowed. They are ignored when computing the
discretization range and are preserved as `NA_integer_` in the returned
score vector.

## Examples

``` r
x <- 0:24

out <- var_discretize(x, n_levels = 10)

out$score
#>  [1]  1  1  1  2  2  3  3  3  4  4  5  5  5  6  6  7  7  8  8  8  9  9 10 10 10
out$midpoints
#>    1    2    3    4    5    6    7    8    9   10 
#>  1.2  3.6  6.0  8.4 10.8 13.2 15.6 18.0 20.4 22.8 
```
