# Get file info for a sequence of experiment files

Grab metadata from files stored in the same folder (e.g. a sequence of
trials in an experiment).

## Usage

``` r
get_wl_metadata(file_path, pattern = "*.ddf")
```

## Arguments

- file_path:

  Path where files are stored. Should be in the same folder.

- pattern:

  Regex pattern for identifying relevant files in the file_path.

## Value

Either a `data.frame` (if a single file is supplied) or a `list` of
`data.frame`s (if a list of files is supplied), with information as
supplied from [`file.info()`](https://rdrr.io/r/base/file.info.html).

## Details

If several files (e.g. successive trials from one experiment) are stored
in one folder, use this function to obtain metadata in a list format.
Runs [`file.info()`](https://rdrr.io/r/base/file.info.html) from base R
to extract info from files.

This function is not truly considered to be part of the batch analysis
pipeline; see
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)
for a similar function that not only grabs metadata but also imports &
analyzes files. Instead, `get_wl_metadata()` is meant to be a handy
function to investigate metadata issues that arise if running
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)
goes awry.

Unlike
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
this function does not necessarily need files to all be work loops. Any
file type is welcome (as long as the Regex `pattern` argument makes
sense).

## See also

[`summarize_wl_trials`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md)

Other data import functions:
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`read_ddf_dir()`](https://docs.ropensci.org/workloopR/reference/read_ddf_dir.md),
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)

Other workloop functions:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

Other batch analyses:
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

## Author

Vikram B. Baliga

## Examples

``` r

library(workloopR)

# get file info for files included with workloopR
wl_meta <- get_wl_metadata(system.file("extdata/wl_duration_trials",
                                       package = 'workloopR'))
```
