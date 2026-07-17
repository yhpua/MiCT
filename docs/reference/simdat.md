# Simulate Longitudinal PROM Data and a Binary Anchor

`simdat()` simulates Time 1 and Time 2 item responses for a 10-item PROM
and a binary anchor / transition rating from an item response theory
context. Each item has four ordered response categories scored 0, 1, 2,
and 3, so the total PROM score ranges from 0 to 30 at each time point.

## Usage

``` r
simdat(
  N = 2000,
  mn_imic = 0.37425,
  sd_imic = 0.05,
  cor_t1_change = -0.5,
  mean_tetch = 0.3,
  sd_tetch = 1,
  rel_trt = 0.7,
  seed = 1234,
  return_latent = TRUE,
  add_change = FALSE
)
```

## Arguments

- N:

  Integer. Sample size for simulation.

- mn_imic:

  Numeric. Mean individual MIC on the latent theta-change scale. For
  context, `mn_imic = 0.5` corresponds approximately to a raw-score MIC
  of about 2.8 points, while `mn_imic = 0.37425` corresponds
  approximately to a raw-score MIC of about 2.5 points on the 0-30 PROM
  scale.

- sd_imic:

  Numeric. Standard deviation of individual MICs on the latent
  theta-change scale.

- cor_t1_change:

  Numeric. Correlation between baseline theta and latent change.

- mean_tetch:

  Numeric. Mean latent change.

- sd_tetch:

  Numeric. Standard deviation of latent change.

- rel_trt:

  Numeric. Target reliability of perceived change used to generate the
  binary anchor / transition rating.

- seed:

  Optional integer. Random seed used to make the simulated data
  reproducible. If `NULL`, the current random-number generator state is
  used.

- return_latent:

  Logical. If `TRUE`, returns latent variables and item parameters in
  the output object.

- add_change:

  Logical. If `TRUE`, adds `change = score_t2 - score_t1` to the
  returned data frame. Defaults to `FALSE`.

## Value

A list containing:

- seed:

  The random seed used.

- settings:

  Simulation settings.

- item_names:

  Names of Time 1 items, Time 2 items, and anchor.

- truth:

  Truth / diagnostic quantities, including `target_rel_trt` and
  `observed_rel_trt`.

- datw:

  The simulated wide-format data frame.

If `return_latent = TRUE`, the output also includes item parameters,
latent variables, perceived change, and individual MICs.

## Details

The returned data frame includes item-level responses, the binary anchor
`trat`, the Time 1 summed PROM score `score_t1`, and the Time 2 summed
PROM score `score_t2`. If `add_change = TRUE`, the observed change score
`change = score_t2 - score_t1` is also added.

The R code is adapted from supplementary materials of Terluin et al.
Qual Life Res. 2024;33:963-973.

## Examples

``` r
sim <- simdat(N = 200, seed = 123, add_change = TRUE)

names(sim$datw)
#>  [1] "item1"    "item2"    "item3"    "item4"    "item5"    "item6"   
#>  [7] "item7"    "item8"    "item9"    "item10"   "item1.1"  "item2.1" 
#> [13] "item3.1"  "item4.1"  "item5.1"  "item6.1"  "item7.1"  "item8.1" 
#> [19] "item9.1"  "item10.1" "trat"     "score_t1" "score_t2" "change"  
sim$truth
#> $target_rel_trt
#> [1] 0.7
#> 
#> $observed_rel_trt
#> [1] 0.7617496
#> 
#> $latent_mic
#> [1] 0.37425
#> 
#> $mean_individual_mic
#> [1] 0.3706512
#> 
#> $sd_individual_mic
#> [1] 0.05012383
#> 
#> $mean_theta_change
#> [1] 0.3
#> 
#> $sd_theta_change
#> [1] 1
#> 
#> $prop_improved
#> [1] 0.48
#> 
#> $cor_change_anchor
#> [1] 0.5584512
#> 
#> $cor_theta_t1_theta_t2
#> [1] 0.5164919
#> 
#> $cor_theta_change_anchor
#> [1] 0.6508751
#> 
#> $raw_score_range
#> [1]  0 30
#> 
```
