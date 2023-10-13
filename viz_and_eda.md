Visualization
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggridges)
```

## Load the weather_df

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USW00022534 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: /Users/shinamin0218/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-10-03 10:19:25.871292 (8.525)

    ## file min/max dates: 1869-01-01 / 2023-09-30

    ## using cached file: /Users/shinamin0218/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-10-03 10:19:44.246716 (3.83)

    ## file min/max dates: 1949-10-01 / 2023-09-30

    ## using cached file: /Users/shinamin0218/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-10-03 10:19:49.571321 (0.994)

    ## file min/max dates: 1999-09-01 / 2023-09-30

``` r
## using cached file: /Users/jeffgoldsmith/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly
## date created (size, mb): 2023-09-19 15:41:55.07359 (8.524)
## file min/max dates: 1869-01-01 / 2023-09-30
## using cached file: /Users/jeffgoldsmith/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly
## date created (size, mb): 2023-09-25 10:06:23.827176 (3.83)
## file min/max dates: 1949-10-01 / 2023-09-30
## using cached file: /Users/jeffgoldsmith/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly
## date created (size, mb): 2023-09-19 15:42:03.139582 (0.994)
## file min/max dates: 1999-09-01 / 2023-09-30

weather_df
```

    ## # A tibble: 2,190 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1  
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6
    ## # ℹ 2,180 more rows

``` r
## # A tibble: 2,190 × 6
##    name           id          date        prcp  tmax  tmin
##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6
##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2
##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1
##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7
##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2
##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1
##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1  
##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7
##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3
## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6
## # ℹ 2,180 more rows
```

## Scatterplot!!

Create my first scatterplot ever.

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

New apporach, same plot.

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Save and edit a plot object.

``` r
weather_plot =
  weather_df %>%
  ggplot(aes(x = tmin, y = tmax))
  
weather_plot + geom_point()
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## ADvanced scatterplot …

Start with the same one and make it fancy!

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point() +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

What about the `aes` placement ..?

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name)) +
  geom_smooth()
```

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Let’s facet some things!!

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax, alpha = tmin, color = name)) +
  geom_point() +
  geom_smooth(se = FALSE, size = 2) +
  facet_grid(. ~ name)
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: The following aesthetics were dropped during statistical transformation: alpha
    ## ℹ This can happen when ggplot fails to infer the correct grouping structure in
    ##   the data.
    ## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
    ##   variable into a factor?
    ## The following aesthetics were dropped during statistical transformation: alpha
    ## ℹ This can happen when ggplot fails to infer the correct grouping structure in
    ##   the data.
    ## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
    ##   variable into a factor?
    ## The following aesthetics were dropped during statistical transformation: alpha
    ## ℹ This can happen when ggplot fails to infer the correct grouping structure in
    ##   the data.
    ## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
    ##   variable into a factor?

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Let’s combine some elements and try a new plot.

``` r
weather_df %>%
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point(aes(size = prcp), alpha = 0.5) +
  geom_smooth(se = FALSE) +
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

## Some small notes

How many geoms have to exist?

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = 0.5) +
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

You can use a `neat` geom!

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = tmax)) +
  geom_density2d() +
  geom_point(alpha = 0.3)
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_density2d()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

## Univariate plots

Histograms are really great.

``` r
weather_df %>%
  ggplot(aes(x = tmin)) +
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite values (`stat_bin()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Can we add color…?

``` r
weather_df %>%
  ggplot(aes(x = tmin)) +
  geom_histogram(position = "dodge") +
  facet_grid(. ~ name)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite values (`stat_bin()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

Let’s try a new geometry!

``` r
weather_df %>%
  ggplot(aes(x = tmin, fill = name)) +
  geom_density(alpha = 0.3)
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_density()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

What about box plots??

``` r
weather_df %>%
  ggplot(aes(x = name, y = tmin)) +
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_boxplot()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

Trendy plots :-)

``` r
weather_df %>%
  ggplot(aes(x = name, y = tmin, fill = name)) +
  geom_violin(alpha = 0.5) +
  stat_summary()
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_ydensity()`).

    ## Warning: Removed 17 rows containing non-finite values (`stat_summary()`).

    ## No summary function supplied, defaulting to `mean_se()`

![](viz_and_eda_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

Ridge plots – the most popular plot of 2017

``` r
weather_df %>%
  ggplot(aes(x = tmin, y = name)) +
  geom_density_ridges()
```

    ## Picking joint bandwidth of 1.41

    ## Warning: Removed 17 rows containing non-finite values
    ## (`stat_density_ridges()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

## Save and Embed

Let’s save a scatterplot.

``` r
weather_plot =
weather_df %>%
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = 0.5)

ggsave("./result/weather_plot.pdf", weather_plot, width = 8, height = 5)
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

What about embedding…

``` r
weather_plot
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

Embed at different size.

``` r
weather_plot
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->
