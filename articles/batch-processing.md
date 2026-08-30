# Batch processing

Many of the functions in the `workloopR` package are built to facilitate
batch processing of workloop and related data files. This vignette will
start with an overview of how the functions were intended to be used for
batch processing and then provide specific examples.

## Conceptual overview

We generally expect a single file to store data from a single
experimental trial, whereas directories hold data from all the trials of
a single experiment. Accordingly, the `muscle_stim` objects created and
used by most of the `workloopR` functions are intended to hold data from
a single trial of a workloop or related experiment. Lists are then used
to package together trials from a single experiment. This also lends
itself to using recursion to transform and analyze all data from a
single experiment.

In broad strokes, there are three ways that batch processing has been
worked into `workloopR` functions. First, some functions like the
`*_dir()` family of import functions and
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md)
specifically generate or require lists of `muscle_stim` objects. Second,
the first argument of all other functions are the objects being
manipulated, which can help clean up recursion using the
[`purrr::map()`](https://purrr.tidyverse.org/reference/map.html) family
of functions. Finally, some functions return summarized data as single
rows of a data.frame that can easily be bound together to generate a
summary table.

## Load packages and data

This vignette will rely heavily on the
[`purrr::map()`](https://purrr.tidyverse.org/reference/map.html) family
of functions for recursion, though it should be mentioned that the
[`base::apply()`](https://rdrr.io/r/base/apply.html) family of functions
would work as well.

``` r

library(workloopR)
library(magrittr)
library(purrr)
```

## Necessarily-multi-trial functions

### `*_dir()` functions

Both
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)
and
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md)
have alternatives suffixed by `_dir()` to read in multiple files from a
directory. Both take a path to the directory and an optional regular
expression to filter files by and return a list of `muscle_stim` objects
or `analyzed_workloop` objects, respectively.

``` r

workloop_trials_list<-
  system.file(
    "extdata/wl_duration_trials",
    package = 'workloopR') %>%
  read_ddf_dir(phase_from_peak = TRUE)

workloop_trials_list[1:2]
#> [[1]]
#> # Workloop Data: 3 channels recorded over 0.3244s
#> File ID: 01_4pulse.ddf
#> 
#>    Time Position   Force Stim
#> 1 1e-04 0.698128 57.2970    0
#> 2 2e-04 0.699741 57.7805    0
#> 3 3e-04 0.699418 58.9095    0
#> 4 4e-04 0.697160 55.8450    0
#> 5 5e-04 0.698773 58.2645    0
#> 6 6e-04 0.698451 57.4580    0
#> # … with 3238 more rows
#> 
#> [[2]]
#> # Workloop Data: 3 channels recorded over 0.3244s
#> File ID: 02_2pulse.ddf
#> 
#>    Time Position   Force Stim
#> 1 1e-04 0.698773 47.9420    0
#> 2 2e-04 0.698451 48.1035    0
#> 3 3e-04 0.700064 48.2645    0
#> 4 4e-04 0.698451 48.5875    0
#> 5 5e-04 0.698773 48.7485    0
#> 6 6e-04 0.699418 49.0710    0
#> # … with 3238 more rows
```

The `sort_by` argument can be used to rearrange this list by any
attribute of the read-in objects. By default, the objects are sorted by
their modification time. Other arguments of
[`read_ddf()`](https://docs.ropensci.org/workloopR/reference/read_ddf.md)
and
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md)
can also be passed to their `*_dir()` alternatives as named arguments.

``` r

analyzed_wl_list<-
  system.file(
    "extdata/wl_duration_trials",
    package = 'workloopR') %>%
  read_analyze_wl_dir(sort_by = 'file_id',
                      phase_from_peak = TRUE,
                      cycle_def = 'lo',
                      keep_cycles = 3)

analyzed_wl_list[1:2]
#> [[1]]
#> File ID: 01_4pulse.ddf
#> Cycles: 1 cycles kept out of 5
#> Mean Work: 0.00274 J
#> Mean Power: 0.07843 W
#> 
#> 
#> [[2]]
#> File ID: 02_2pulse.ddf
#> Cycles: 1 cycles kept out of 5
#> Mean Work: 0.00098 J
#> Mean Power: 0.02783 W
```

### Summarizing workloop trials

In a series of workloop trials, it can useful to see how mean power and
work change as you vary different experimental parameters. To facilitate
this,
[`summarize_wl_trials()`](https://docs.ropensci.org/workloopR/reference/summarize_wl_trials.md)
specifically takes a list of `analyzed_workloop` objects and returns a
`data.frame` of this information. We will explore ways of generating
lists of analyzed workloops without using
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)
in the following section.

``` r

analyzed_wl_list %>%
  summarize_wl_trials
#>         File_ID Cycle_Frequency Amplitude  Phase Stimulus_Pulses
#> 1 01_4pulse.ddf              28      3.15 -24.36               4
#> 2 02_2pulse.ddf              28      3.15 -24.64               2
#> 3 03_6pulse.ddf              28      3.15 -24.92               6
#> 4 04_4pulse.ddf              28      3.15 -24.64               4
#>   Stimulus_Frequency      mtime     Mean_Work  Mean_Power
#> 1                300 1788074122  0.0027387056 0.078427135
#> 2                300 1788074122  0.0009849216 0.027832717
#> 3                300 1788074122 -0.0002192395 0.004323004
#> 4                300 1788074122  0.0022793831 0.065468837
```

## Manual recursion examples

### Batch import for non-ddf data

One of the more realistic use cases for manual recursion is for
importing data from multiple related trials that are not stored in ddf
format. As with importing individual non-ddf data sources, we start by
reading the data into a data.frame, only now we want a list of
data.frames. In this example, we will read in csv files and stitch them
into a list using
[`purrr::map()`](https://purrr.tidyverse.org/reference/map.html)

``` r

non_ddf_list<-
  # Generate a vector of file names
  system.file(
    "extdata/twitch_csv",
    package = 'workloopR') %>%
  list.files(full.names = T) %>%
  # Read into a list of data.frames
  map(read.csv) %>%
  # Coerce into a workloop object
  map(as_muscle_stim, type = "twitch")
```

### Data transformation and analysis

Applying a constant transformation to a list of `muscle_stim` objects is
fairly straightforward using
[`purrr::map()`](https://purrr.tidyverse.org/reference/map.html).

``` r

non_ddf_list<-
  non_ddf_list %>%
  map(~{
    attr(.x,"stimulus_width")<-0.2
    attr(.x,"stimulus_offset")<-0.1
    return(.x)
  }) %>%
  map(fix_GR,2)
```

Applying a non-constant transformation like setting a unique file ID can
be done using
[`purrr::map2()`](https://purrr.tidyverse.org/reference/map2.html).

``` r

file_ids<-paste0("0",1:4,"-",2:5,"mA-twitch.csv")

non_ddf_list<-
  non_ddf_list %>%
  map2(file_ids, ~{
    attr(.x,"file_id")<-.y
    return(.x)
  })

non_ddf_list
#> [[1]]
#> # Twitch Data: 3 channels recorded over 0.4001s
#> File ID: 01-2mA-twitch.csv
#> 
#>    Time  Position   Force Stim
#> 1 1e-04 -3.002651 474.262    0
#> 2 2e-04 -3.001682 471.682    0
#> 3 3e-04 -3.001360 472.650    0
#> 4 4e-04 -3.000554 471.037    0
#> 5 5e-04 -3.001199 472.004    0
#> 6 6e-04 -3.001360 472.327    0
#> # … with 3995 more rows
#> 
#> [[2]]
#> # Twitch Data: 3 channels recorded over 0.4001s
#> File ID: 02-3mA-twitch.csv
#> 
#>    Time  Position   Force Stim
#> 1 1e-04 -3.002489 476.520    0
#> 2 2e-04 -3.001199 475.553    0
#> 3 3e-04 -3.000876 474.585    0
#> 4 4e-04 -3.001199 473.940    0
#> 5 5e-04 -3.001199 474.262    0
#> 6 6e-04 -3.001360 474.262    0
#> # … with 3995 more rows
#> 
#> [[3]]
#> # Twitch Data: 3 channels recorded over 0.4001s
#> File ID: 03-4mA-twitch.csv
#> 
#>    Time  Position   Force Stim
#> 1 1e-04 -3.002651 451.037    0
#> 2 2e-04 -3.001360 449.747    0
#> 3 3e-04 -3.000715 449.747    0
#> 4 4e-04 -3.001037 449.424    0
#> 5 5e-04 -3.000876 449.424    0
#> 6 6e-04 -3.001521 450.069    0
#> # … with 3995 more rows
#> 
#> [[4]]
#> # Twitch Data: 3 channels recorded over 0.4001s
#> File ID: 04-5mA-twitch.csv
#> 
#>    Time  Position   Force Stim
#> 1 1e-04 -3.002327 446.521    0
#> 2 2e-04 -3.001521 445.876    0
#> 3 3e-04 -3.001199 445.876    0
#> 4 4e-04 -3.001199 445.876    0
#> 5 5e-04 -3.001360 445.553    0
#> 6 6e-04 -3.001037 446.199    0
#> # … with 3995 more rows
```

Analysis can similarly be run recursively.
[`isometric_timing()`](https://docs.ropensci.org/workloopR/reference/isometric_timing.md)
in particular returns a single row of a data.frame with timings and
forces for key points in an isometric dataset. Here we can use
[`purrr::map_dfr()`](https://purrr.tidyverse.org/reference/map_dfr.html)
to bind the rows together for neatness.

``` r

non_ddf_list %>%
  map_dfr(isometric_timing)
#>             file_id time_stim force_stim time_peak force_peak time_rising_10
#> 1 01-2mA-twitch.csv    0.1002    480.391    0.1153    654.258         0.1049
#> 2 02-3mA-twitch.csv    0.1002    461.682    0.1149    748.772         0.1050
#> 3 03-4mA-twitch.csv    0.1002    450.069    0.1145    799.416         0.1049
#> 4 04-5mA-twitch.csv    0.1002    448.134    0.1141    824.899         0.1048
#>   force_rising_10 time_rising_90 force_rising_90 time_relaxing_90
#> 1         497.810         0.1118         637.484           0.1216
#> 2         492.326         0.1109         720.708           0.1207
#> 3         488.778         0.1106         764.901           0.1201
#> 4         488.778         0.1107         787.803           0.1198
#>   force_relaxing_90 time_relaxing_50 force_relaxing_50
#> 1           637.807           0.1348           567.486
#> 2           721.031           0.1325           605.872
#> 3           764.901           0.1314           624.904
#> 4           788.126           0.1311           636.517
```
