# All-in-one import function for work loop files

`read_analyze_wl()` is an all-in-one function to read in a work loop
file, select cycles, and compute work and power output.

## Usage

``` r
read_analyze_wl(file_name, ...)
```

## Arguments

- file_name:

  A .ddf file that contains data from a single workloop experiment

- ...:

  Additional arguments to be passed to
  [`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md),
  [`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
  or
  [`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md).

## Value

The function returns a `list` of class `analyzed_workloop` that provides
instantaneous velocity, a smoothed velocity, and computes work,
instantaneous power, and net power from a work loop experiment. All data
are organized by the cycle number and important metadata are stored as
Attributes.

Within the `list`, each entry is labeled by cycle and includes:

- Time:

  Time, in sec

- Position:

  Length change of the muscle, corrected for gear ratio, in mm

- Force:

  Force, corrected for gear ratio, in mN

- Stim:

  When stimulation occurs, on a binary scale

- Cycle:

  Cycle ID, as a letter

- Inst_velocity:

  Instantaneous velocity, computed from `Position` change, reported in
  meters/sec

- Filt_velocity:

  Instantaneous velocity, after low-pass filtering, again in meter/sec

- Inst_Power:

  Instantaneous power, a product of `Force` and `Filt_velocity`,
  reported in J

- Percent_of_Cycle:

  The percent of that particular cycle which has elapsed

In addition, the following information is stored in the
`analyzed_workloop` object's attributes:

- stimulus_frequency:

  Frequency at which stimulus pulses occurred

- cycle_frequency:

  Frequency of oscillations (assuming sine wave trajectory)

- total_cycles:

  Total number of oscillatory cycles (assuming sine wave trajectory)
  that the muscle experienced.

- cycle_def:

  Specifies what part of the cycle is understood as the beginning and
  end. There are currently three options: 'lo' for L0-to-L0; 'p2p' for
  peak-to-peak; and 't2t' for trough-to-trough

- amplitude:

  Amplitude of length change (assuming sine wave trajectory)

- phase:

  Phase of the oscillatory cycle (in percent) at which stimulation
  occurred. Somewhat experimental, please use with caution

- position_inverted:

  Logical; whether position inversion has been applied)

- units:

  The units of measurement for each column in the object after running
  this function. See Warning

- sample_frequency:

  Frequency at which samples were collected

- header:

  Additional information from the header

- units_table:

  Units from each Channel of the original ddf file

- protocol_table:

  Protocol in tabular format; taken from the original ddf file

- stim_table:

  Specific info on stimulus protocol; taken from the original ddf file

- stimulus_pulses:

  Number of sequential pulses within a stimulation train

- stimulus_offset:

  Timing offset at which stimulus began

- gear_ratio:

  Gear ratio applied by this function

- file_id:

  File name

- mtime:

  Time at which file was last modified

- retained_cycles:

  Which cycles were retained, as numerics

- summary:

  Simple table showing work (in J) and net power (in W) for each cycle

## Details

Please be careful with units! See Warnings below. This function combines
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)
with
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md)
and then ultimately
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md)
into one handy function.

As detailed in these three functions, possible arguments include:  
`cycle_def` - used to specify which part of the cycle is understood as
the beginning and end. There are currently three options: 'lo' for
L0-to-L0; 'p2p' for peak-to-peak; and 't2t' for trough-to-trough  
`bworth_order` - Filter order for low-pass filtering of `Position` via
[`signal::butter`](https://rdrr.io/pkg/signal/man/butter.html) prior to
finding peak lengths. Default: 2.  
`bworth_freq` - Critical frequency (scalar) for low-pass filtering of
`Position` via
[`signal::butter`](https://rdrr.io/pkg/signal/man/butter.html) prior to
finding peak lengths. Default: 0.05.  
`keep_cycles` - Which cycles should be retained. Default: 4:6.  
`GR` - Gear ratio. Default: 1.  
`M` - Velocity multiplier used to positivize velocity; should be either
-1 or 1. Default: -1.  
`vel_bf` - Critical frequency (scalar) for low-pass filtering of
velocity via
[`signal::butter`](https://rdrr.io/pkg/signal/man/butter.html). Default:
0.05.  

The gear ratio (GR) and velocity multiplier (M) parameters can help
correct for issues related to the magnitude and sign of data collection.
By default, they are set to apply no gear ratio adjustment and to
positivize velocity. Instantaneous velocity is often noisy and the
`vel_bf` parameter allows for low-pass filtering of velocity data. See
[`signal::butter()`](https://rdrr.io/pkg/signal/man/butter.html) and
[`signal::filtfilt()`](https://rdrr.io/pkg/signal/man/filtfilt.html) for
details of how filtering is achieved.

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

[`read_ddf`](https://docs.ropensci.org/workloopR/reference/read_ddf.md),
[`select_cycles`](https://docs.ropensci.org/workloopR/reference/select_cycles.md)
[`analyze_workloop`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md)

Other data analyses:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`isometric_timing()`](https://docs.ropensci.org/workloopR/reference/isometric_timing.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)

Other data import functions:
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_ddf_dir()`](https://docs.ropensci.org/workloopR/reference/read_ddf_dir.md),
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)

Other workloop functions:
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md),
[`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)

## Author

Vikram B. Baliga

## Examples

``` r

library(workloopR)

# import the workloop.ddf file included in workloopR and analyze with
# a gear ratio correction of 2 and cycle definition of peak-to-peak
wl_dat <- read_analyze_wl(system.file("extdata", "workloop.ddf",
                                      package = 'workloopR'),
                          phase_from_peak = TRUE,
                          GR = 2, cycle_def = "p2p")

```
