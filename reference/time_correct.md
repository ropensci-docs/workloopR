# Time correction for work loop experiments

Correct for potential degradation of muscle over time.

## Usage

``` r
time_correct(x)
```

## Arguments

- x:

  A `data.frame` with summary data, e.g. an object created by
  [`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md).

## Value

A `data.frame` that additionally contains:

- Time_Corrected_Work :

  Time corrected work output, transformed from `$Mean_Work`

- Time_Corrected_Power :

  Time corrected net power output, transformed from `$Mean_Power`

And new attributes:

- power_difference :

  Difference in mass-specific net power output between the final and
  first trials.

- time_difference :

  Difference in mtime between the final and first trials.

- time_correction_rate :

  Overall rate; `power_difference` divided by `time_difference`.

## Details

This function assumes that across a batch of successive trials, the
stimulation parameters for the first and final trials are identical. If
not, DO NOT USE. Decline in power output is therefore assumed to be a
linear function of time. Accordingly, the difference between the final
and first trial's (absolute) power output is used to 'correct' trials
that occur in between, with explicit consideration of run order and time
elapsed (via mtime). A similar correction procedure is applied to work.

## See also

[`summarize_wl_trials`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md)

Other workloop functions:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md)

Other batch analyses:
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md)

## Author

Vikram B. Baliga and Shreeram Senthivasan

## Examples

``` r

library(workloopR)

# batch read and analyze files included with workloopR
analyzed_wls <- read_analyze_wl_dir(system.file("extdata/wl_duration_trials",
                                                package = 'workloopR'),
                                    phase_from_peak = TRUE,
                                    cycle_def = "p2p", keep_cycles = 2:4)

# now summarize
summarized_wls <- summarize_wl_trials(analyzed_wls)


# mtimes within the package are not accurate, so we'll supply
# our own vector of mtimes
summarized_wls$mtime <- read.csv(
                          system.file(
                            "extdata/wl_duration_trials/ddfmtimes.csv",
                            package="workloopR"))$mtime

# now time correct
timecor_wls <- time_correct(summarized_wls)
timecor_wls
#>         File_ID Cycle_Frequency Amplitude  Phase Stimulus_Pulses
#> 1 01_4pulse.ddf              28      3.15 -24.36               4
#> 2 02_2pulse.ddf              28      3.15 -24.64               2
#> 3 03_6pulse.ddf              28      3.15 -24.92               6
#> 4 04_4pulse.ddf              28      3.15 -24.64               4
#>   Stimulus_Frequency      mtime     Mean_Work   Mean_Power Time_Corrected_Work
#> 1                300 1409014596  0.0028362363  0.078967198        0.0028362363
#> 2                300 1409014778  0.0009686570  0.026247519        0.0010905501
#> 3                300 1409015053 -0.0001310863 -0.004017894        0.0001749861
#> 4                300 1409015235  0.0024082708  0.066959552        0.0028362363
#>   Time_Corrected_Power
#> 1          0.078967198
#> 2          0.029667538
#> 3          0.004569734
#> 4          0.078967198

```
