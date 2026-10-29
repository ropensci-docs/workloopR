# Compute timing and magnitude of force in isometric trials

Calculate timing and magnitude of force at stimulation, peak force, and
various parts of the rising (force development) and relaxation (falling)
phases of the twitch.

## Usage

``` r
isometric_timing(x, rising = c(10, 90), relaxing = c(90, 50))
```

## Arguments

- x:

  A `muscle_stim` object that contains data from an isometric twitch
  trial, ideally created via `read_ddf`.

- rising:

  Set points of the rising phase to be described. By default: 10% and
  90%.

- relaxing:

  Set points of the relaxation phase to be described. By default: 90%
  and 50%.

## Value

A `data.frame` with the following metrics as columns:

- file_ID :

  File ID

- time_stim:

  Time between beginning of data collection and when stimulation occurs

- force_stim:

  Magnitude of force at the onset of stimulation

- time_peak:

  Absolute time of peak force, i.e. time between beginning of data
  collection and when peak force occurs

- force_peak:

  Magnitude of peak force

- time_rising_X:

  Time between beginning of data collection and X% of force development

- force_rising_X:

  Magnitude of force at X% of force development

- time_relaxing_X:

  Time between beginning of data collection and X% of force relaxation

- force_relaxing_X:

  Magnitude of force at X% of relaxation

## Details

The `data.frame` (x) must have time series data organized in columns.
Generally, it is preferred that you use a `muscle_stim` object imported
by
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md).

The `rising` and `relaxing` arguments allow for the user to supply
numeric vectors of any length. By default, these arguments are
`rising = c(10, 90)` and `relaxing = c(90, 50)`. Numbers in each of
these correspond to percent values and capture time and force at that
percent of the corresponding curve. These values can be replaced by
those that the user specifies and do not necessarily need to have length
= 2. But please note that 0 and 100 should not be used, e.g.
`rising = seq(10, 90, 5)` works, but `rising = seq(0, 100, 5)` does not.

## References

Ahn AN, and Full RJ. 2002. A motor and a brake: two leg extensor muscles
acting at the same joint manage energy differently in a running insect.
Journal of Experimental Biology 205, 379-389.

## See also

Other data analyses:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md)

Other twitch functions:
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md)

## Author

Vikram B. Baliga

## Examples

``` r

library(workloopR)

# import the twitch.ddf file included in workloopR
twitch_dat <-read_ddf(system.file("extdata", "twitch.ddf",
                                  package = 'workloopR'))

# run isometric_timing() to get info on twitch kinetics
# we'll use different set points than the defaults
analyze_twitch <- isometric_timing(twitch_dat,
  rising = c(25, 50, 75),
  relaxing = c(75, 50, 25)
)

# see the results
analyze_twitch
#>      file_id time_stim force_stim time_peak force_peak time_rising_25
#> 1 twitch.ddf    0.1002    224.067    0.1141   412.4495         0.1057
#>   force_rising_25 time_rising_50 force_rising_50 time_rising_75 force_rising_75
#> 1         273.743          0.107             321         0.1087        366.1605
#>   time_relaxing_75 force_relaxing_75 time_relaxing_50 force_relaxing_50
#> 1           0.1241          365.5155           0.1311          318.2585
#>   time_relaxing_25 force_relaxing_25
#> 1            0.143           271.808
```
