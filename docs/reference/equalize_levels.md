# Equalize Paired Item Response Levels

`equalize_levels()` prepares paired Time 1 and Time 2 PROM items for
longitudinal CFA by ensuring that each item has the same observed
response levels at both time points. Sparse or mismatched response
categories are collapsed consistently within each item pair.

## Usage

``` r
equalize_levels(
  data,
  pair_by = c("position", "suffix"),
  t1_suffix = NULL,
  t2_suffix = NULL,
  min_resp = 5L,
  verbose = TRUE,
  print_tables = TRUE
)
```

## Arguments

- data:

  A data frame containing paired item variables. Do not include the
  transition rating variable.

- pair_by:

  Pairing method. `"position"` assumes the first half of columns are
  Time 1 items and the second half are Time 2 items. `"suffix"` detects
  item pairs using `t1_suffix` and `t2_suffix`.

- t1_suffix:

  Regex suffix identifying Time 1 items when `pair_by = "suffix"`. Use
  `""` when Time 1 items have no suffix.

- t2_suffix:

  Regex suffix identifying Time 2 items when `pair_by = "suffix"`.

- min_resp:

  Integer. Minimum number of responses required in each response
  category at both time points. Categories with fewer responses are
  collapsed.

- verbose:

  Logical. If `TRUE`, prints progress messages.

- print_tables:

  Logical. If `TRUE`, prints before/after frequency tables only for item
  pairs that required collapsing or equalization.

## Value

A list with components:

- data:

  The processed data frame, ordered as T1 items followed by T2 items.

- pair_map:

  A data frame describing matched T1/T2 item pairs.

- collapsed_items:

  A data frame listing item pairs that required collapsing/equalization.

- before_tables:

  Frequency tables before collapsing for affected item pairs.

- after_tables:

  Frequency tables after collapsing for affected item pairs.

- mappings:

  Category mappings for affected item pairs.

- summary:

  A data frame summarizing all item pairs.

## Details

Item pairs can be detected either by column position or by suffix
patterns. The function does not shift or rescale item scores; it only
collapses and equalizes response levels within paired items.

## Examples

``` r
dat <- data.frame(
  item1 = c(1, 1, 2, 2, 3, 3, 3, 4),
  item2 = c(1, 2, 2, 3, 3, 3, 4, 4),
  item1.1 = c(1, 2, 2, 2, 3, 4, 4, 4),
  item2.1 = c(1, 1, 2, 2, 3, 4, 4, 4)
)

out <- equalize_levels(
  data = dat,
  pair_by = "suffix",
  t1_suffix = "",
  t2_suffix = "\\.1",
  min_resp = 2,
  verbose = FALSE,
  print_tables = FALSE
)
```
