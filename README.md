
[![Travis-CI Build Status](https://travis-ci.org/hypertidy/ncmeta.svg?branch=master)](https://travis-ci.org/hypertidy/ncmeta) [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/hypertidy/ncmeta?branch=master&svg=true)](https://ci.appveyor.com/project/hypertidy/ncmeta) [![CRAN status](http://www.r-pkg.org/badges/version/ncmeta)](https://cran.r-project.org/package=ncmeta) [![Coverage Status](https://img.shields.io/codecov/c/github/hypertidy/ncmeta/master.svg)](https://codecov.io/github/hypertidy/ncmeta?branch=master)

<!-- README.md is generated from README.Rmd. Please edit that file -->
ncmeta
======

Straightforward NetCDF metadata for use in building interfaces to this format.

Installation
------------

You can install ncmeta from github with:

``` r
# install.packages("devtools")
devtools::install_github("hypertidy/ncmeta")
```

Example
-------

This is a basic example which shows you how to find out the basic information on structures in a NetCDF file:

``` r
library(ncmeta)
filename <- system.file("extdata", "S2008001.L3m_DAY_CHL_chlor_a_9km.nc", package = "ncmeta")
nc_inq(filename) # one-row summary of file

nc_dim(filename, 0)  ## first dimesion
nc_dims(filename)  ## all dimensions
```

Compare timings.

``` r
library(ncmeta)
f <- system.file("extdata", "S2008001.L3m_DAY_CHL_chlor_a_9km.nc", package = "ncmeta")
u <- "https://upwell.pfeg.noaa.gov/erddap/tabledap/FRDCPSTrawlLHHaulCatch"

library(microbenchmark)
junk <- capture.output(a <- microbenchmark(nc_open = ncdf4::nc_open(f), 
               ncdump = ncdump::NetCDF(f), 
               ncmeta = nc_meta(f) ,
               RNetCDF = RNetCDF::print.nc(RNetCDF::open.nc(f)), 
                times = 10)
)
print(a)
#> Unit: milliseconds
#>     expr       min        lq      mean    median        uq        max
#>  nc_open  14.76244  15.77639  16.19893  16.53470  16.65902   17.10492
#>   ncdump 100.62247 104.08135 237.69712 106.14621 111.95256 1314.25375
#>   ncmeta  68.73485  70.90362  84.83429  71.20265  73.63348  199.00647
#>  RNetCDF  36.83804  37.73866  48.54410  39.96898  44.43127  108.39011
#>  neval
#>     10
#>     10
#>     10
#>     10
```
