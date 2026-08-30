# Select cycles from a work loop object

Retain data from a work loop experiment based on position cycle

## Usage

``` r
select_cycles(
  x,
  cycle_def,
  keep_cycles = 4:6,
  bworth_order = 2,
  bworth_freq = 0.05,
  ...
)
```

## Arguments

- x:

  A `workloop` object (see Details for how it should be organized)

- cycle_def:

  A string specifying how cycles should be defined; one of: "lo", "p2p",
  or "t2t". See Details more info

- keep_cycles:

  The indices of the cycles to keep. Include 0 to keep data identified
  as being outside complete cycles

- bworth_order:

  Filter order for low-pass filtering of `Position` via
  [`signal::butter()`](https://rdrr.io/pkg/signal/man/butter.html) prior
  to finding L0

- bworth_freq:

  Critical frequency (scalar) for low-pass filtering of `Position` via
  [`signal::butter()`](https://rdrr.io/pkg/signal/man/butter.html) prior
  to finding L0

- ...:

  Additional arguments passed to/from other functions that make use of
  `select_cycles()`

## Value

A `workloop` object with rows subsetted by the chosen position cycles. A
`Cycle` column is appended to denote which cycle each time point is
associated with. Finally, all attributes from the input `workloop`
object are retained and one new attribute is added to record which
cycles from the original data were retained.

## Details

`select_cycles()` subsets data from a workloop trial by position cycle.
The `cycle_def` argument is used to specify which part of the cycle is
understood as the beginning and end. There are currently three
options:  
'lo' for L0-to-L0;  
'p2p' for peak-to-peak; and  
't2t' for trough-to-trough  

Peaks are identified using
[`pracma::findpeaks()`](https://rdrr.io/pkg/pracma/man/findpeaks.html).
L0 points on the rising edge are found by finding the midpoints between
troughs and the following peak. However the first and last extrema and
L0 points may be misidentified by this method. Please plot your
`Position` cycles to ensure the edge cases are identified correctly.

The `keep_cycles` argument is used to determine which cycles (as defined
by `cycle_def` should be retained in the final dataset. Zero is the
index assigned to all data points that are determined to be outside a
complete cycle.

The `muscle_stim` object (`x`) must be a `workloop`, preferably read in
by one of our data import functions. Please see documentation for
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md)
if you need to manually construct a `muscle_stim` object from another
source.

## See also

[`analyze_workloop`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`read_analyze_wl`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`read_analyze_wl_dir`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)

Other data transformations:
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md)

Other workloop functions:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

## Author

Vikram B. Baliga and Shreeram Senthivasan

## Examples

``` r

library(workloopR)

# import the workloop.ddf file included in workloopR
wl_dat <-read_ddf(system.file("extdata", "workloop.ddf",
                              package = 'workloopR'),
                  phase_from_peak = TRUE)

# select cycles 3 through 5 via the peak-to-peak definition
wl_selected <- select_cycles(wl_dat, cycle_def = "p2p", keep_cycles = 3:5)


# are the cycles of (approximately) the same length?
summary(as.factor(wl_selected$Cycle))
#>   a   b   c 
#> 357 357 357 
```
