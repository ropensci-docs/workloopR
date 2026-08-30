# Package index

## Data import functions

Import data from .ddf or other file types

- [`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)
  : Import work loop or isometric data from .ddf files
- [`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md)
  : All-in-one import function for work loop files
- [`as_muscle_stim()`](https://docs.ropensci.org/workloopR/reference/as_muscle_stim.md)
  : Create your own muscle_stim object

## Data transformation functions

Manipulation of `muscle_stim` objects prior to analyses

- [`select_cycles()`](https://docs.ropensci.org/workloopR/reference/select_cycles.md)
  : Select cycles from a work loop object
- [`invert_position()`](https://docs.ropensci.org/workloopR/reference/invert_position.md)
  : Invert the position data
- [`fix_GR()`](https://docs.ropensci.org/workloopR/reference/fix_GR.md)
  : Adjust for the gear ratio of a motor arm

## Analysis functions

Functions for the analysis of a single `muscle_stim` object

- [`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md)
  : Analyze work loop object to compute work and power output
- [`isometric_timing()`](https://docs.ropensci.org/workloopR/reference/isometric_timing.md)
  : Compute timing and magnitude of force in isometric trials
- [`trapezoidal_integration()`](https://docs.ropensci.org/workloopR/reference/trapezoidal_integration.md)
  : Approximate the definite integral via the trapezoidal rule
- [`time_correct()`](https://docs.ropensci.org/workloopR/reference/time_correct.md)
  : Time correction for work loop experiments

## Batch analysis functions

Functions for the analysis of multiple files or `muscle_stim` objects

- [`read_ddf_dir()`](https://docs.ropensci.org/workloopR/reference/read_ddf_dir.md)
  : Import a batch of work loop or isometric data files from a directory
- [`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)
  : Read and analyze work loop files from a directory
- [`get_wl_metadata()`](https://docs.ropensci.org/workloopR/reference/get_wl_metadata.md)
  : Get file info for a sequence of experiment files
- [`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md)
  : Summarize work loop files
