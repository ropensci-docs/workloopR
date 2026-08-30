# Approximate the definite integral via the trapezoidal rule

Mostly meant for internal use in our analysis functions, but made
available for other use cases. Accordingly, it does not strictly rely on
objects of class `muscle_stim`.

## Usage

``` r
trapezoidal_integration(x, f)
```

## Arguments

- x:

  a variable, e.g. vector of positions

- f:

  integrand, e.g. vector of forces

## Value

A numerical value indicating the value of the integral.

## Details

In the functions
[`analyze_workloop()`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`read_analyze_wl()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md)
, and
[`read_analyze_wl_dir()`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md),
work is calculated as the difference between the integral of the upper
curve and the integral of the lower curve of a work loop.

## References

Atkinson, Kendall E. (1989), An Introduction to Numerical Analysis (2nd
ed.), New York: John Wiley & Sons

## See also

[`analyze_workloop`](https://docs.ropensci.org/workloopR/reference/analyze_workloop.md),
[`read_analyze_wl`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl.md),
[`read_analyze_wl_dir`](https://docs.ropensci.org/workloopR/reference/read_analyze_wl_dir.md)

## Author

Vikram B. Baliga

## Examples

``` r

# create a circle centered at (x = 10, y = 20) with radius 2
t <- seq(0, 2 * pi, length = 1000)
coords <- t(rbind(10 + sin(t) * 2, 20 + cos(t) * 2))


# use the function to get the area
trapezoidal_integration(coords[, 1], coords[, 2])
#> [1] 12.56629

# does it match (pi * r^2)?
3.14159265358 * (2^2) # very close
#> [1] 12.56637
```
