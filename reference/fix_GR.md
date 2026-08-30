# Adjust for the gear ratio of a motor arm

Fix a discrepancy between the gear ratio of the motor arm used and the
gear ratio recorded by software.

## Usage

``` r
fix_GR(x, GR = 1)
```

## Arguments

- x:

  A `muscle_stim` object

- GR:

  Gear ratio, set to 1 by default

## Value

An object of the same class(es) as the input (`x`). The function will
multiply `Position` by (1/GR) and multiply `Force` by GR, returning an
object with new values in `$Position` and `$Force`. Other columns and
attributes are welcome and will simply be passed on unchanged into the
resulting object.

## Details

The `muscle_stim` object can be of any type, including `workloop`,
`twitch`, or `tetanus`.

If you have manually constructed the object via
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md),
the `muscle_stim` object should have columns as follows:  
`Position`: length change of the muscle;  
`Force`: force  

## See also

[`analyze_workloop`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`read_analyze_wl`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`read_analyze_wl_dir`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)

Other data transformations:
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md)

Other workloop functions:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

Other twitch functions:
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`isometric_timing()`](https://docs.ropensci.org/workloopR/reference/isometric_timing.md)

Other tetanus functions:
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md)

## Author

Vikram B. Baliga

## Examples

``` r

library(workloopR)

# import the workloop.ddf file included in workloopR
wl_dat <-read_ddf(system.file("extdata", "workloop.ddf",
                              package = 'workloopR'),
                  phase_from_peak = TRUE)

# apply a gear ratio correction of 2
# this will multiply Force by 2 and divide Position by 2
wl_fixed <- fix_GR(wl_dat, GR = 2)

# quick check:
max(wl_fixed$Force) / max(wl_dat$Force) # 5592.578 / 2796.289 = 2
#> [1] 2
max(wl_fixed$Position) / max(wl_dat$Position) # 1.832262 / 3.664524 = 0.5
#> [1] 0.5
```
