
# biscale <img src="man/figures/logo.png" align="right" />

[![lifecycle](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)
[![Travis-CI Build Status](https://travis-ci.com/slu-openGIS/biscale.svg?branch=master)](https://travis-ci.com/slu-openGIS/biscale)
[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/slu-openGIS/biscale?branch=master&svg=true)](https://ci.appveyor.com/project/chris-prener/biscale)
[![Coverage status](https://codecov.io/gh/slu-openGIS/biscale/branch/master/graph/badge.svg)](https://codecov.io/github/slu-openGIS/biscale?branch=master)
[![CRAN_status_badge](http://www.r-pkg.org/badges/version/biscale)](https://cran.r-project.org/package=biscale)
[![cran checks](https://cranchecks.info/badges/worst/biscale)](https://cran.r-project.org/web/checks/check_results_biscale.html)
[![Downloads](http://cranlogs.r-pkg.org/badges/biscale?color=brightgreen)](http://www.r-pkg.org/pkg/biscale)
[![DOI](https://zenodo.org/badge/183024212.svg)](https://zenodo.org/badge/latestdoi/183024212)

`biscale` implements a set of functions for bivariate thematic mapping based on the [tutorial](https://timogrossenbacher.ch/2019/04/bivariate-maps-with-ggplot2-and-sf/) written by Timo Grossenbacher and Angelo Zehr as well as a set of bivariate mapping palettes from Joshua Stevens’s [tutorial](http://www.joshuastevens.net/cartography/make-a-bivariate-choropleth-map/). The package currently supports two-by-two and three-by-three bivariate maps:

<img src="man/figures/biscale.001.png" width="100%" />

In addition to support for both two-by-two and three-by-three maps, the package also supports four methods for calculating breaks for bivariate maps.

## What’s New on CRAN?

### Dependency Change

In order to resolve a conflict between the `tibble` package and `biscale`, the `sf` package is now a required dependency as opposed to being a suggested dependency.

### Mapping Points

The development version contains a new function, `bi_scale_color()`, which replicates the bivariate mapping workflow for point and line data. We don’t have any sample data for it yet, but the workflow mapping point data looks like this:

``` r
# create classes
data <- bi_class(pointData, x = xvar, y = yvar, style = "quantile", dim = 3)

# create map
map <- ggplot() +
  geom_sf(data = pointData, mapping = aes(color = bi_class), show.legend = FALSE) +
  bi_scale_color(pal = "DkBlue", dim = 3) +
  bi_theme()
```

The creation of classes works the same way. The only difference is (a) the use of the `color` (or `colour`) argument in the aesthetic mapping for `geom_sf()` and the use of `bi_scale_color()` afterwards!

### Manual Palettes

If you want to use a different palette with `biscale` plots, you can use the new `bi_pal_manual()` function to create the plot and then apply it to `bi_scale_fill()` or `bi_scale_color()` using the `pal` argument.

``` r
# create classes
data <- bi_class(pointData, x = xvar, y = yvar, style = "quantile", dim = 2)

# create custom palette
custom_pal <- bi_pal_manual(val_1_1 = "#E8E8E8", val_1_2 = "#73AE80", val_2_1 = "#6C83B5", val_2_2 = "#2A5A5B")

# create map
map <- ggplot() +
  geom_sf(data = pointData, mapping = aes(color = bi_class), show.legend = FALSE) +
  bi_scale_color(pal = custom_pal, dim = 3) +
  bi_theme()
```

## Quick Start

If the `sf` package is already installed, the development version of `biscale` can be accessed from GitHub with `remotes`:

``` r
install.packages("biscale")
```

Alternatively, the development version of `biscale` can be accessed from GitHub with `remotes`:

``` r
# install.packages("remotes")
remotes::install_github("slu-openGIS/biscale")
```

Additional details, including some tips for installing `sf`, can be found in the [Get started article](articles/biscale.html#getting-started).

## Resources

In addition to instructions for installation, the main [Get started](articles/biscale.html) article has:

  - a quick overview of bivariate mapping,
  - a description of the workflow for creating bivariate maps,
  - a comparison of different approaches to calculating those classes,
  - and a comparison of different color palettes for bivariate mapping.
