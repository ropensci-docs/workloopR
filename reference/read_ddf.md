# Import work loop or isometric data from .ddf files

`read_ddf` reads in workloop, twitch, or tetanus experiment data from
.ddf files.

## Usage

``` r
read_ddf(
  file_name,
  file_id = NA,
  rename_cols = list(c(2, 3), c("Position", "Force")),
  skip_cols = 4:11,
  phase_from_peak = FALSE,
  ...
)
```

## Arguments

- file_name:

  A .ddf file that contains data from a single workloop, twitch, or
  tetanus experiment

- file_id:

  A string identifying the experiment. The file name is used by default.

- rename_cols:

  List consisting of a vector of indices of columns to rename and a
  vector of new column names. See Details.

- skip_cols:

  Numeric vector of column indices to skip. See Details.

- phase_from_peak:

  Logical, indicating whether percent phase of stimulation should be
  recorded relative to peak length or relative to L0 (default)

- ...:

  Additional arguments passed to/from other functions that work with
  `read_ddf()`

## Value

An object of class `workloop`, `twitch`, or `tetanus`, all of which
inherit class `muscle_stim`. These objects behave like `data.frames` in
most situations but also store metadata from the ddf as attributes.

The `muscle_stim` object's columns contain:

- Time:

  Time

- Position:

  Length change of the muscle, uncorrected for gear ratio

- Force:

  Force, uncorrected for gear ratio

- Stim:

  When stimulation occurs, on a binary scale

In addition, the following information is stored in the `data.frame`'s
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
[`read_ddf_dir()`](https://docs.ropensci.org/workloopR/reference/read_ddf_dir.md)

## Author

Vikram B. Baliga and Shreeram Senthivasan

## Examples

``` r

library(workloopR)

# import the workloop.ddf file included in workloopR
wl_dat <-read_ddf(system.file("extdata", "workloop.ddf",
                              package = 'workloopR'),
                  phase_from_peak = TRUE)

```
