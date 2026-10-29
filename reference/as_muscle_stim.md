# Create your own muscle_stim object

For use when data are not stored in .ddf format and you would like to
create a `muscle_stim` object that can be used by other workloopR
functions.

## Usage

``` r
as_muscle_stim(x, type, sample_frequency, ...)
```

## Arguments

- x:

  A `data.frame`. See Details for how it should be organized.

- type:

  Experiment type; must be one of: "workloop", "tetanus", or "twitch."

- sample_frequency:

  Numeric value of the frequency at which samples were recorded; must be
  in Hz. Please format as numeric, e.g. `10000` works but `10000 Hz`
  does not

- ...:

  Additional arguments that can be passed in as attributes. See Details.

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

`muscle_stim` objects, which are required by (nearly) all workloopR
functions, are automatically created via
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md).
Should you have data that are stored in a format other than .ddf, use
this function to create your own object of class `muscle_stim`.

The input `x` must be a `data.frame` that contains time series of
numeric data collected from an experiment. Each row must correspond to a
sample, and these columns (exact title matches) must be included:  
"Time" - time, recorded in seconds  
"Position" - instantaneous position of the muscle, preferably in
millimeters  
"Force" - force, preferably in millinewtons  
"Stim" - whether stimulation has occurred. All entries must be either 0
(no stimulus) or 1 (stimulus occurrence).

Additional arguments can be provided via `...`. For all experiment
types, the following attributes are appropriate:  
"units","header", "units_table", "protocol_table", "stim_table",
"stimulus_pulses", "stimulus_offset", "stimulus_width", "gear_ratio",
"file_id", or "mtime".

Please ensure that further attributes are appropriate to your experiment
type.

For workloops, these include: "stimulus_frequency", "cycle_frequency",
"total_cycles", "cycle_def", "amplitude", "phase", and
"position_inverted"

For twitches or tetanic trials: "stimulus_frequency", and
"stimulus_length"

## See also

[`read_ddf`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)

Other data import functions:
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`read_ddf_dir()`](https://docs.ropensci.org/workloopR/reference/read_ddf_dir.md),
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)

## Author

Shreeram Senthivasan

## Examples

``` r

library(workloopR)

# import the workloop.ddf file included in workloopR
wl_dat <-read_ddf(system.file("extdata", "workloop.ddf",
                              package = 'workloopR'))

```
