# Analyze work loop object to compute work and power output

Compute work and power output from a work loop experiment on a per-cycle
basis.

## Usage

``` r
analyze_workloop(x, simplify = FALSE, GR = 1, M = -1, vel_bf = 0.05, ...)
```

## Arguments

- x:

  A `workloop` object of class `muscle_stim` that has been passed
  through `select_cycles`. See Details.

- simplify:

  Logical. If `FALSE`, the full analyzed workloop object is returned. If
  `TRUE` a simpler table of net work and power (by cycle) is returned.

- GR:

  Gear ratio, set to 1 by default

- M:

  Velocity multiplier, set adjust the sign of velocity. This parameter
  should generally be either -1 (the default) or 1.

- vel_bf:

  Critical frequency (scalar) for low-pass filtering of velocity via
  [`signal::butter()`](https://rdrr.io/pkg/signal/man/butter.html)

- ...:

  Additional arguments potentially passed down from
  [`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md)
  or
  [`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)

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

Please note that
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md)
must be run on data prior to using this function. This function relies
on the input `muscle_stim` object being organized by cycle number.

The `muscle_stim` object (`x`) must be a `workloop`, preferably read in
by one of our data import functions. Please see documentation for
[`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md)
if you need to manually construct a `muscle_stim` object from a non .ddf
source.

The gear ratio (GR) and velocity multiplier (M) parameters can help
correct for issues related to the magnitude and sign of data collection.
By default, they are set to apply no gear ratio adjustment and to
positivize velocity. Instantaneous velocity is often noisy and the
`vel_bf` parameter allows for low-pass filtering of velocity data. See
[`signal::butter()`](https://rdrr.io/pkg/signal/man/butter.html) and
[`signal::filtfilt()`](https://rdrr.io/pkg/signal/man/filtfilt.html) for
details of how filtering is achieved.

Please also be careful with units! Se Warning section below.

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
[`read_analyze_wl`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`select_cycles`](https://docs.ropensci.org/workloopR/reference/select_cycles.md)

Other data analyses:
[`isometric_timing()`](https://docs.ropensci.org/workloopR/reference/isometric_timing.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md)

Other workloop functions:
[`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md),
[`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md),
[`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md),
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md),
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

# run the analysis function and get the full object
wl_analyzed <- analyze_workloop(wl_selected, GR = 2)

# print methods give a short summary
print(wl_analyzed)
#> File ID: workloop.ddf
#> Cycles: 3 cycles kept out of 6
#> Mean Work: 0.00308 J
#> Mean Power: 0.08474 W
#> 

# summary provides a bit more detail
summary(wl_analyzed)
#> # Workloop Data:
#> 
#> 
#> File ID: workloop.ddf
#> Mod Time (mtime): 2026-08-30 07:15:22
#> Sample Frequency: 10000Hz
#> 
#> data.frame Columns: 
#>   Position (mm)
#>   Force (mN)
#>   Stim (TTL)
#>   Cycle (letters)
#>   Inst_Velocity (m/s)
#>   Filt_Velocity (m/s)
#>   Inst_Power (W)
#>   Percent_of_Cycle (NA)
#> 
#> Stimulus Offset: 0.012s
#> Stimulus Frequency: 300Hz
#> Stimulus Width: 0.2ms
#> Stimulus Pulses: 4
#> Gear Ratio: 2
#> 
#> Cycle Frequency: 28Hz
#> Total Cycles (peak-to-peak): 6
#> Cycles Retained: 3
#> Amplitude: 1.575mm
#> 
#> 
#>   Cycle        Work  Net_Power
#> a     A 0.002785397 0.07639783
#> b     B 0.003147250 0.08661014
#> c     C 0.003305744 0.09122522

# run the analysis but get the simplified version
wl_analyzed_simple <- analyze_workloop(wl_selected, simplify = TRUE, GR = 2)
```
