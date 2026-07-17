# Extract details from a SIM threshold object

`sim_threshold_details()` extracts additional details from an object
returned by
[`sim_threshold()`](https://yhpua.github.io/MiCT/reference/sim_threshold.md),
including the fitted lavaan model, CFA data, latent anchor location,
model-implied category probabilities, and bootstrap results.

## Usage

``` r
sim_threshold_details(x)
```

## Arguments

- x:

  A `sim_threshold` object.

## Value

A list of additional model details.
