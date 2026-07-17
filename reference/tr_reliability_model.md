# Generate CFA syntax for anchor reliability

`tr_reliability_model()` generates lavaan syntax for estimating the
reliability of an anchor or transition rating item using confirmatory
factor analysis.

## Usage

``` r
tr_reliability_model(
  data,
  anchor = NULL,
  xsec = FALSE,
  pair_by = c("position", "suffix"),
  t1_suffix = NULL,
  t2_suffix = NULL,
  factor_names = NULL,
  print_model = TRUE
)
```

## Arguments

- data:

  A data frame containing items and an anchor variable.

- anchor:

  Character. Name of the anchor variable. If `NULL`, the final column of
  `data` is treated as the anchor.

- xsec:

  Logical. If `TRUE`, generate a cross-sectional one-factor model. If
  `FALSE`, generate a longitudinal two-factor model.

- pair_by:

  Pairing method for longitudinal data. `"position"` assumes Time 1
  items followed by Time 2 items. `"suffix"` detects pairs using
  `t1_suffix` and `t2_suffix`.

- t1_suffix:

  Regex suffix identifying Time 1 items when `pair_by = "suffix"`. Use
  `""` when Time 1 items have no suffix.

- t2_suffix:

  Regex suffix identifying Time 2 items when `pair_by = "suffix"`.

- factor_names:

  Optional character vector of factor name(s). If `NULL`, defaults to
  `"F1"` when `xsec = TRUE`, and `c("F1", "F2")` when `xsec = FALSE`.

- print_model:

  Logical. If `TRUE`, print the generated lavaan syntax.

## Value

An object of class `tr_reliability_model`. The lavaan syntax can be
accessed using `$model`.

## Details

The generated model follows the approach described by Griffiths et al. J
Clin Epidemiol. 2022;141:36-45. No constraints are placed on loadings or
thresholds. For longitudinal data, residuals of corresponding items
across time-points are allowed to correlate to account for item
non-independence.

For cross-sectional data, a one-factor model is generated. For
longitudinal data, a two-factor model is generated, with the anchor
loading on both Time 1 and Time 2 factors.

## Examples

``` r
sim <- simdat(N = 200, seed = 123)
dat <- sim$datw

tr_reliability_model(
  data = dat[, c(sim$item_names$t1_items,
                 sim$item_names$t2_items,
                 "trat")],
  anchor = "trat",
  xsec = FALSE,
  pair_by = "suffix",
  t1_suffix = "",
  t2_suffix = "\\.1"
)
#> 
#> ================ ANCHOR RELIABILITY CFA MODEL ================
#> 
#> 
#> # Factors
#> F1 =~ item1 + item2 + item3 + item4 + item5 + item6 + item7 + item8 + item9 + item10 + trat
#> F2 =~ item1.1 + item2.1 + item3.1 + item4.1 + item5.1 + item6.1 + item7.1 + item8.1 + item9.1 + item10.1 + trat
#> 
#> # Correlated errors over time
#> item1 ~~ item1.1
#> item2 ~~ item2.1
#> item3 ~~ item3.1
#> item4 ~~ item4.1
#> item5 ~~ item5.1
#> item6 ~~ item6.1
#> item7 ~~ item7.1
#> item8 ~~ item8.1
#> item9 ~~ item9.1
#> item10 ~~ item10.1
#> 
#> ===============================================================
```
