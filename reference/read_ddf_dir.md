# Import a batch of work loop or isometric data files from a directory

Uses
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)
to read in workloop, twitch, or tetanus experiment data from multiple
.ddf files.

## Usage

``` r
read_ddf_dir(file_path, pattern = "*.ddf", sort_by = "mtime", ...)
```

## Arguments

- file_path:

  Path where files are stored. Should be in the same folder.

- pattern:

  Regex pattern for identifying relevant files in the file_path.

- sort_by:

  Metadata by which files should be sorted to be in the correct run
  order. Defaults to `mtime`, which is time of last modification of
  files.

- ...:

  Additional arguments to be passed to
  [`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md).

## Value

A list of objects of class `workloop`, `twitch`, or `tetanus`, all of
which inherit class `muscle_stim`. These objects behave like
`data.frames` in most situations but also store metadata from the ddf as
attributes.

Each `muscle_stim` object's columns contain:

- Time:

  Time

- Position:

  Length change of the muscle, uncorrected for gear ratio

- Force:

  Force, uncorrected for gear ratio

- Stim:

  When stimulation occurs, on a binary scale

In addition, the following information is stored in each `data.frame`'s
attributes:

- sample_frequency:

  Frequency at which samples were collected

- pulses:

  Number of sequential pulses within a stimulation train

- total_cycles_lo:

  Total number of oscillatory cycles (assuming sine wave trajectory)
  that the muscle experienced. Cycles are defined with respect to
  initial muscle length (L0-to-L0 as opposed to peak-to-peak).

- amplitude:

  amplitude of length change (again, assuming sine wave trajectory)

- cycle_frequency:

  Frequency of oscillations (again, assuming sine wave trajectory)

- units:

  The units of measurement for each column in the `data.frame`. This
  might be the most important attribute so please check that it makes
  sense!

## Details

Read in a .ddf file that contains data from an experiment. If position
and force do not correspond to columns 2 and 3 (respectively), replace
"2" and "3" within `rename_cols` accordingly. Similarly,
`skip_cols = 4:11` should be adjusted if more than 11 columns are
present and/or columns 4:11 contain important data.

Please note that there is no correction for gear ratio or further
manipulation of data. See `fix_GR` to adjust gear ratio. Gear ratio can
also be adjusted prior to analyses within the
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md)
function, the data import all-in-one function
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
or the batch analysis all-in-one
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md).

Please also note that organization of data within the .ddf file is
assumed to conform to that used by Aurora Scientific's Dynamic Muscle
Control and Analysis Software. YMMV if using a .ddf file from another
source. The
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md)
function can be used to generate `muscle_stim` objects if data are
imported via another function. Please feel free to contact us with any
issues or requests.

## See also

Other data import functions:
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)

## Author

Vikram B. Baliga and Shreeram Senthivasan

## Examples

``` r

library(workloopR)

# import a set of twitch .ddf files included in workloopR
workloop_dat <-read_ddf_dir(system.file("extdata/wl_duration_trials",
                 package = 'workloopR'))
```
