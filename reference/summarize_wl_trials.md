# Summarize work loop files

Summarize important info from work loop files stored in the same folder
(e.g. a sequence of trials in an experiment) including experimental
parameters, run order, and `mtime`.

## Usage

``` r
summarize_wl_trials(wl_list)
```

## Arguments

- wl_list:

  List of `analyzed_workloop` objects, preferably one created by
  [`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md).

## Value

A `data.frame` of information about the collection of workloop files.
Columns include:

- File_ID :

  Name of the file

- Cycle_Frequency :

  Frequency of Position change

- Amplitude :

  amplitude of Position change

- Phase :

  Phase of the oscillatory cycle (in percent) at which stimulation
  occurred. Somewhat experimental, please use with caution

- Stimulus_Pulses :

  Number of stimulation pulses

- mtime :

  Time at which file's contents were last changed (`mtime`)

- Mean_Work :

  Mean work output from the selected cycles

- Mean_Power :

  Net power output from the selected cycles

## Details

If several files (e.g. successive trials from one experiment) are stored
in one folder, use this function to obtain summary stats and metadata
and other parameters. This function requires a list of
`analyze_workloop` objects, which can be readily obtained by first
running
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)
on a specified directory.

## References

Josephson RK. 1985. Mechanical Power output from Striated Muscle during
Cyclic Contraction. Journal of Experimental Biology 114: 493-512.

## See also

[`read_analyze_wl_dir`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`get_wl_metadata`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`time_correct`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

Other workloop functions:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

Other batch analyses:
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

## Author

Vikram B. Baliga and Shreeram Senthivasan

## Examples

``` r

library(workloopR)

# batch read and analyze files included with workloopR
analyzed_wls <- read_analyze_wl_dir(system.file("extdata/wl_duration_trials",
                                               package = 'workloopR'),
                                    phase_from_peak = TRUE,
                                    cycle_def = "p2p",
                                    keep_cycles = 2:4
                                    )

# now summarize
summarized_wls <- summarize_wl_trials(analyzed_wls)
```
