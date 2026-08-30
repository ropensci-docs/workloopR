# Read and analyze work loop files from a directory

All-in-one function to import multiple workloop .ddf files from a
directory, sort them by mtime, analyze them, and store the resulting
objects in an ordered list.

## Usage

``` r
read_analyze_wl_dir(file_path, pattern = "*.ddf", sort_by = "mtime", ...)
```

## Arguments

- file_path:

  Directory in which files are located

- pattern:

  Regular expression used to specify files of interest. Defaults to all
  .ddf files within file_path

- sort_by:

  Metadata by which files should be sorted to be in the correct run
  order. Defaults to `mtime`, which is time of last modification of
  files.

- ...:

  Additional arguments to be passed to
  [`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
  [`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
  [`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
  or
  [`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md).

## Value

A list containing `analyzed_workloop` objects, one for each file that is
imported and subsequently analyzed. The list is sorted according to the
`sort_by` parameter, which by default uses the time of last modification
of each file's contents (mtime).

## Details

Work loop data files will be imported and then arranged in the order in
which they were run (assuming run order is reflected in `mtime`).
Chiefly used in conjunction with
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md)
and
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)
if time correction is desired.

## Warning

Most systems we have encountered record Position data in millimeters and
Force in millinewtons, and therefore this function assumes data are
recorded in those units. Through a series of internal conversions, this
function computes velocity in meters/sec, work in Joules, and power in
Watts. If your raw data do not originate in millimeters and
millinewtons, please transform your data accordingly and ignore what you
see in the attribute `units`.

## References

Josephson RK. 1985. Mechanical Power output from Striated Muscle during
Cyclic Contraction. Journal of Experimental Biology 114: 493-512.

## See also

[`read_analyze_wl`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`get_wl_metadata`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`summarize_wl_trials`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

Other data analyses:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`isometric_timing()`](https://docs.ropensci.org/workloopR/reference/isometric_timing.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md)

Other data import functions:
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`read_ddf_dir()`](https://docs.ropensci.org/workloopR/reference/read_ddf_dir.md),
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)

Other workloop functions:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

Other batch analyses:
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

## Author

Shreeram Senthivasan

## Examples

``` r

library(workloopR)

# batch read and analyze files included with workloopR
analyzed_wls <- read_analyze_wl_dir(system.file("extdata/wl_duration_trials",
                                                package = 'workloopR'),
                                    phase_from_peak = TRUE,
                                    cycle_def = "p2p", keep_cycles = 2:4)
```
