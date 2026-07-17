# Changelog

## MiCT 2.0.0

### Package history

`MiCT` is the renamed and substantially extended successor to the
original `MIC` package created by Iris Eekhout. Version 2.0.0 reflects
the package rename and major methodological updates.

### Major changes

- Added
  [`sim_threshold()`](https://yhpua.github.io/MiCT/reference/sim_threshold.md)
  for LCFA-based threshold estimation for single-item measures.
- Added
  [`mim_threshold()`](https://yhpua.github.io/MiCT/reference/mim_threshold.md)
  for LCFA-based threshold estimation for multi-item questionnaires.
- Added
  [`sim_mic_lcfa()`](https://yhpua.github.io/MiCT/reference/sim_mic_lcfa.md)
  for LCFA-based MIC estimation for single-item measures.
- Updated
  [`mic_lcfa()`](https://yhpua.github.io/MiCT/reference/mic_lcfa.md)
  with CFA/probit expected-score mapping  
- Added
  [`equalize_levels()`](https://yhpua.github.io/MiCT/reference/equalize_levels.md),
  [`var_discretize()`](https://yhpua.github.io/MiCT/reference/var_discretize.md)
  and
  [`lcfa_model()`](https://yhpua.github.io/MiCT/reference/lcfa_model.md)  
- Renamed [`simdat()`](https://yhpua.github.io/MiCT/reference/simdat.md)
  output `empirical_rel_trt` to `observed_rel_trt`.
- Updated documentation, examples, and tests.
