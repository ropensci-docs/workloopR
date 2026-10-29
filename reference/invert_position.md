# Invert the position data

Multiply instantaneous position by -1.

## Usage

``` r
invert_position(x)
```

## Arguments

- x:

  A `muscle_stim` object

## Value

A `workloop` object with inverted position. The `position_inverted`
attribute is set to `TRUE` and all others are retained.

## Details

The `muscle_stim` object can be of any type, including `workloop`,
`twitch`, or `tetanus`.

If you have manually constructed the object via
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md),
the `muscle_stim` object should have a column entitled `Position`. Other
columns and attributes are welcome and will be passed along unchanged.

## See also

Other data transformations:
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md)

Other workloop functions:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

Other twitch functions:
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`isometric_timing()`](https://docs.ropensci.org/workloopR/reference/isometric_timing.md)

Other tetanus functions:
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md)

## Author

Vikram B. Baliga

## Examples

``` r

library(workloopR)

# import the workloop.ddf file included in workloopR
wl_dat <-read_ddf(system.file("extdata", "workloop.ddf",
                              package = 'workloopR'),
                  phase_from_peak = TRUE)

# invert the sign of Position
wl_fixed <- invert_position(wl_dat)

# quick check:
max(wl_fixed$Position) / min(wl_dat$Position) # -1
#> [1] -1
```
