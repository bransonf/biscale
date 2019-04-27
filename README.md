
<!-- README.md is generated from README.Rmd. Please edit that file -->

# biscale

[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![Travis-CI Build
Status](https://travis-ci.org/slu-openGIS/biscale.svg?branch=master)](https://travis-ci.org/slu-openGIS/biscale)
[![AppVeyor Build
Status](https://ci.appveyor.com/api/projects/status/github/slu-openGIS/biscale?branch=master&svg=true)](https://ci.appveyor.com/project/chris-prener/biscale)
[![Coverage
status](https://codecov.io/gh/slu-openGIS/biscale/branch/master/graph/badge.svg)](https://codecov.io/github/slu-openGIS/biscale?branch=master)
[![CRAN\_status\_badge](http://www.r-pkg.org/badges/version/biscale)](https://cran.r-project.org/package=biscale)

`biscale` implements a set of functions for bivariate themeatic mapping
based on the
[tutorial](https://timogrossenbacher.ch/2019/04/bivariate-maps-with-ggplot2-and-sf/)
written by Timo Grossenbacher and Angelo Zehr as well as a set of
bivariate mapping palettes from Joshua Stevens’s
[tutorial](http://www.joshuastevens.net/cartography/make-a-bivariate-choropleth-map/).

## Installation

### Installing Dependencies

You should check the [`sf` package
website](https://r-spatial.github.io/sf/) for the latest details on
installing dependencies for that package. Instructions vary
significantly by operating system. Linux users in particular should also
check the [`biscale` package
repo](https://github.com/slu-openGIS/biscale/blob/master/.travis/install.sh)
for a script that installs the relevant dependencies. For best results,
have `sf` installed before you install `biscale`. Other dependencies,
like `dplyr`, will be installed automatically with `areal` if they are
not already present.

### Installing biscale

Once you have `sf` installed, you can install `biscale` with the
`remotes` package:

``` r
# install.packages("remotes")
remotes::install_github("slu-openGIS/biscale")
```

## Usage

Creating bivariate plots in the style described by [Grossenbacher and
Zehr](https://timogrossenbacher.ch/2019/04/bivariate-maps-with-ggplot2-and-sf/)
requires a number of dependencies in addition to `biscale` - `ggplot2`
for plotting, `cowplot` for combining the legend and the main map, and
`sf` for working with spaital objects in `R`:

``` r
# load dependencies
library(biscale)
library(ggplot2)
library(cowplot)
library(sf)
```

The `biscale` package comes with some sample data from St. Louis, MO
that you can use to check out the bivariate mapping workflow:

``` r
# load data
data <- stl_race_income
```

First, we want to create our classes. `biscale` currently supports a 3x3
table of classes, created with the `bi_class()` function:

``` r
# create classes
data <- bi_class(data, x = pctWhite, y = medInc)
```

The default method for calculating breaks is `"tercile"`, which will
provide breaks at 33.33% and 66.66% percent (i.e. 3 quantile breaks).
Other options are `"equal"`, `"fisher"`, and `"jenks"`.

Once breaks are created, we can use `bi_scale_fill()` as part of our
`ggplot()` call:

``` r
# create map
map <- ggplot() +
  geom_sf(data = data, aes(fill = bi_class), color = "white", size = 0.1, show.legend = FALSE) +
  bi_scale_fill(pal = "DkBlue") +
  labs(
    title = "Race and Income in St. Louis, MO",
    subtitle = "Dark Blue (DkBlue) Palette"
  ) +
  bi_theme()
```

Other options for palettes include `"Brown"`, `"DkCyan"`, `"DkViolet"`,
and `"GrPink"`. The `bi_theme()` function applies a simple theme without
distracting elements, which is preferable given the already elevated
complexity of a bivarite map.

To add a legend to our map, we need to create a second `ggplot` object.
We can use `bi_legend()` to accomplish this, which allows us to easily
specify the fill palette, the x and y axis labels, and their size:

    legend <- bi_legend(pal = "GrPink",
                        xlab = "Higher % White ",
                        ylab = "Higher Income ",
                        size = 8)

Note that
[`plotmath`](https://stat.ethz.ch/R-manual/R-devel/library/grDevices/html/plotmath.html)
is used to draw the arrows since Unicode arrows are font dependent. This
happens internally as part of `bi_legend()` - you don’t need to include
them in your `xlab` and `ylab` arguments\!

With our legend drawn, we can then combine the legend and the map with
`cowplot`. The values needed for this stage will be subject to
experimentation depending on the shape of the map itself.

    # combine map with legend
    finalPlot <- ggdraw() +
      draw_plot(map, 0, 0, 1, 1) +
      draw_plot(legend, 0.2, .7, 0.2, 0.2)
    
    # print map
    finalPlot

![](man/figures/dkblue.png)

## Palettes

Palette options include `"Brown"`, `"DkBlue"`, `"DkCyan"`, `"DkViolet"`,
and `"GrPink"`.

![](man/figures/preview.png)

## Contributor Code of Conduct

Please note that this project is released with a [Contributor Code of
Conduct](.github/CODE_OF_CONDUCT.md). By participating in this project
you agree to abide by its terms.
