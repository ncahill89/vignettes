The Bayesian Transfer Function
================

## Installation

Note the package will not work unless you have the JAGS (Just Another
Gibbs Sampler) software installed on your machine. You can download and
install JAGS from
[here](https://sourceforge.net/projects/mcmc-jags/files/JAGS/4.x/Windows/)
for Windows and
[here](https://sourceforge.net/projects/mcmc-jags/files/JAGS/4.x/Mac%20OS%20X/)
for MAC.

``` r
devtools::install_github("ncahill89/BTFr")
```

You can then load the BTFr package using the `library` function.

``` r
library(BTFr)
```

## Introduction

In this vignette we will run the Bayesian Transfer Function (BTFr). There
are options to use the package default which contains data for New
Jersey, USA. Alternatively, you can supply your own data. When supplying
data, use the package defaults as templates for formatting.

**Example data from New Jersey, USA**

``` r
## Example modern elevation
BTFr::NJ_modern_elevation
```

    ## # A tibble: 175 × 3
    ##    Site               SampleID     SWLI
    ##    <chr>              <chr>       <dbl>
    ##  1 Dodge Garage       DG/09/ST 4   196.
    ##  2 Dodge Garage       DG/09/ST 5   184.
    ##  3 Dodge Garage       DG/09/ST 6   181.
    ##  4 Dodge Garage       DG/09/ST 7   189.
    ##  5 Dodge Garage       DG/09/ST 8   189.
    ##  6 Dodge Garage       DG/09/ST 9   191.
    ##  7 Dodge Garage       DG/09/ST 10  185.
    ##  8 Dodge Garage       DG/09/ST 11  189.
    ##  9 Dodge Garage       DG/09/ST 12  163.
    ## 10 Brigantine Barrier BB/07/ST 1   215.
    ## # … with 165 more rows

``` r
## Example modern species data
BTFr::NJ_modern_species
```

    ## # A tibble: 175 × 23
    ##    `Jm+Bp` `Ti+Sl`    Tc    Hs    Mf    Pl    Pi    Am    Ai    Ea    Tq    Mp
    ##      <dbl>   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1      32      82    10   128     0     4     0     2     0     0     0     2
    ##  2       9      77    21    74     9    11     0    36    16     0     0     4
    ##  3       7     150    25    19    13    12     0    19     0     0     0     1
    ##  4       7      98    32    67     5     9     0    19     2     0     0     0
    ##  5       8     129    39    56     3     6     0    13     1     0     0     2
    ##  6      10     146    25    41     2    17     0     0     0     0     0     0
    ##  7      31      80     7     5    89     0     0     0     0     0     0     0
    ##  8      11      74    26     1     2     2     0    16     0     0     0     0
    ##  9      11      27     9     3    17     1     0   130    16     0     0     0
    ## 10      50     119     5    37     1     0     0    16     0     0     0     0
    ## # … with 165 more rows, and 11 more variables: Calc <dbl>, Rs <dbl>, Ab <dbl>,
    ## #   As <dbl>, Ts <dbl>, Ad <dbl>, TO <dbl>, PH <dbl>, TG <dbl>, Tt <dbl>,
    ## #   Ph <dbl>

``` r
## Example core species data
BTFr::NJ_core_species
```

    ## # A tibble: 69 × 24
    ##    Depth `Jm+Bp` `Ti+Sl`    Tc    Hs    Mf    Pl    Pi    Am    Ai    Ea    Tq
    ##    <dbl>   <dbl>   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1     3      90      11    52     1    34     0     0     8     0     0     0
    ##  2     5      60      31    45     0    37     0     0     1     0     0     0
    ##  3     6      70       6    25     0     4     0     0     0     0     0     0
    ##  4     7      85      12    32     0    14     0     0     0     0     0     0
    ##  5    10      56      32    40     1     8     0     0     0     0     0     0
    ##  6    11      49      21    39     0     0     0     0     0     0     0     0
    ##  7    13      90       9    37     0     0     0     0     0     0     0     0
    ##  8    15      16      44    46     0     0     0     0     1     0     0     0
    ##  9    17      24      22    64     0     0     0     0     2     0     0     0
    ## 10    19      12      31    61     0     0     0     0     1     0     0     0
    ## # … with 59 more rows, and 12 more variables: Mp <dbl>, Calc <dbl>, Rs <dbl>,
    ## #   Ab <dbl>, As <dbl>, Ts <dbl>, Ad <dbl>, TO <dbl>, PH <dbl>, TG <dbl>,
    ## #   Tt <dbl>, Ph <dbl>

``` r
## Example core priors
BTFr::NJ_priors
```

    ## # A tibble: 69 × 3
    ##    Depth prior_lwr prior_upr
    ##    <dbl>     <dbl>     <dbl>
    ##  1     3         0       300
    ##  2     5         0       300
    ##  3     6         0       300
    ##  4     7         0       300
    ##  5    10         0       300
    ##  6    11         0       300
    ##  7    13         0       300
    ##  8    15         0       300
    ##  9    17         0       300
    ## 10    19         0       300
    ## # … with 59 more rows

## Run the modern calibration model

This function takes arguments `modern_elevation` and `modern_species`.
In the example below the package data for New Jersey is used. This will
take \~50 minutes for the default data.

``` r
modern_mod <- run_modern(modern_elevation = NJ_modern_elevation,
                         modern_species = NJ_modern_species)
```

Save the `modern_mod` object to avoid having to rerun this part of the
model again. You can save it with the name of your choosing.

``` r
saveRDS(modern_mod,file = "NJ_modern_BTFr.rds")
```

To read it in again you use

``` r
modern_mod <- readRDS(file = "NJ_modern_BTFr.rds")
```

## Species Response Curves (SRC)

The following will allow you to look at the species response curves.
This function will also return the data used to generate the curves.
This will return response curves for all species.

``` r
src <- response_curves(modern_mod)
```

Add the `select_species` argument to choose specific species you want to
get the curves for.

``` r
src <- response_curves(modern_mod,
                       species_select = c("Ab","Al","Am","Hs","Jm+Bp","Mf","Mp","Pl","Tc","Tl+Sl"))
```

**Plot the SRCs**

``` r
src$src_plot
```

![](BTFr_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

**Get raw relative abundance data**

``` r
src$src_empirical_dat
```

    ## # A tibble: 1,400 × 3
    ##     SWLI species proportion
    ##    <dbl> <chr>        <dbl>
    ##  1  196. Mf         0      
    ##  2  196. Tc         0.0385 
    ##  3  196. Jm+Bp      0.123  
    ##  4  196. Hs         0.492  
    ##  5  196. Am         0.00769
    ##  6  196. Ab         0      
    ##  7  196. Mp         0.00769
    ##  8  196. Pl         0.0154 
    ##  9  184. Mf         0.0350 
    ## 10  184. Tc         0.0817 
    ## # … with 1,390 more rows

**Get model relative abundances**

``` r
src$src_model_dat
```

    ## # A tibble: 1,400 × 5
    ##     SWLI species proportion proportion_lwr proportion_upr
    ##    <dbl> <chr>        <dbl>          <dbl>          <dbl>
    ##  1  27.0 Mf        0.508      0.0214              0.966  
    ##  2  27.0 Tc        0.0445     0.000513            0.273  
    ##  3  27.0 Jm+Bp     0.0262     0.000103            0.175  
    ##  4  27.0 Hs        0.00333    0.00000453          0.0209 
    ##  5  27.0 Am        0.0404     0.000205            0.265  
    ##  6  27.0 Ab        0.312      0.00343             0.912  
    ##  7  27.0 Mp        0.0124     0.0000461           0.0763 
    ##  8  27.0 Pl        0.000200   0.0000000398        0.00138
    ##  9  30.1 Mf        0.510      0.0236              0.960  
    ## 10  30.1 Tc        0.0443     0.000517            0.261  
    ## # … with 1,390 more rows

## Run the reconstruction model

Here we will get a reconstruction for the New Jersey Core provided as
part of the package data. When this is finished it will provide some
convergence diagnostics. You want to see the statement “The accuracy of
the parameter estimation is adequate” to know everything has converged
properly with the model.

``` r
NJ_core_species <- BTFr::NJ_core_species

core_mod <- BTFr::run_core(modern_mod,
                          core_species = NJ_core_species)
```

Save the `core_mod` object to avoid having to rerun this part of the
model again. You can save it with the name of your choosing.

``` r
saveRDS(core_mod,file = "NJ_core_BTFr.rds")
```

To read it in again you use

``` r
core_mod <- readRDS(file = "NJ_core_BTFr.rds")
```

## Get reconstruction results

The following will return a data frame of SWLI estimates with
uncertainties and a corresponding plot of the results.

``` r
swli_res <- swli_results(core_mod)
```

**Get the results**

``` r
swli_res$SWLI_dat
```

    ## # A tibble: 69 × 5
    ##    Depth  SWLI sigma lower upper
    ##    <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1     3  165.  39.4  86.0  244.
    ##  2     5  165.  39.8  85.2  244.
    ##  3     6  150.  51.1  48.1  253.
    ##  4     7  169.  54.8  59.6  279.
    ##  5    10  185.  50.6  84.0  286.
    ##  6    11  198.  44.2 110.   287.
    ##  7    13  202.  45.1 112.   292.
    ##  8    15  187.  38.6 110.   264.
    ##  9    17  187.  38.1 111.   263.
    ## 10    19  185.  37.1 111.   259.
    ## # … with 59 more rows

**Get a plot of the results**

``` r
swli_res$p_SWLI
```

![](BTFr_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

If you wish to include priors for elevation then you can provide the
prior information with the `prior_el` argument. Below is an example
using the priors for New Jersey that are provided as part of the package
data.

``` r
NJ_priors <- BTFr::NJ_priors

core_mod <- BTFr::run_core(modern_mod,
                          core_species = NJ_core_species,
                          prior_el = NJ_priors)
```

## Using your own data

The first thing you need to do is to make sure your data is formatted
correctly and then read it in to R. Note, I use the `readr` package.
Here I will read in some data from Newfoundland.

``` r
library(readr)

NFLD_modern_species <- read_csv("NFLD_modern_species.csv")

NFLD_elevation <- read_csv("NFLD_elevation.csv")

NFLD_core_species <- read_csv("NFLD_core_species.csv")
```

Once you have read in your data you can proceed with running the BTFr.

``` r
modern_mod <- run_modern(modern_elevation = NFLD_elevation,
                         modern_species = NFLD_modern_species)

saveRDS(modern_mod, file = "NFLD_modern_mod.rds")
```

**Get response curves etc.**

``` r
src <- response_curves(modern_mod,
                       species_select = c("Jm+Bp","Mf"))
src$src_plot
```

![](BTFr_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

## Run the reconstruction model

``` r
core_mod <- run_core(modern_mod,
                     core_species = NFLD_core_species)

saveRDS(core_mod, file = "NFLD_core_BTFr.rds")
```

**Look at the results**

``` r
swli_res <- swli_results(core_mod)
swli_res$p_SWLI
```

![](BTFr_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->

## Run a validation

This step should be taken if you have a new modern calibration dataset
that has not previously been validated with the BTFr. If you want to run
a single validation with 90% training data and 10% test data
(recommended for very large datasets) then run the code below.

``` r
valid_run <- run_valid(modern_elevation = NFLD_elevation, 
                       modern_species = NFLD_modern_species)
```

Save the `valid_run` object. You can save it with the name of your
choosing.

``` r
saveRDS(valid_run, file = "valid_run_NFLD.rds")
```

To read it in again you use

``` r
valid_run <- readRDS("valid_run_NFLD.rds")
```

An example of how to summarise the validation results. The coverage
provides the % of the time the True SWLI falls within the model 95%
credible intervals. The Root Mean Squared Error (RMSE) provides a
measurement of prediction error.

``` r
library(dplyr)
valid_run %>% summarise(coverage = sum(lower < True & True < upper)*100/n(),
                        RMSE = sqrt(mean((True - SWLI)^2))) %>% round(2)
```

    ## # A tibble: 1 × 2
    ##   coverage  RMSE
    ##      <dbl> <dbl>
    ## 1      100  22.4

An example of how to visualise the validation results.

``` r
library(ggplot2)
ggplot(valid_run, aes(x = True, y = SWLI)) +
  geom_point() + 
  geom_errorbar(aes(ymin = lower, ymax = upper)) +
  geom_line(aes(x = True, y = True))
```

![](BTFr_files/figure-gfm/unnamed-chunk-33-1.png)<!-- -->

If you want to run a full 10-fold cross validation to get an out of
sample prediction for all the modern samples, then run the code below.
Note this is computationally expensive (expect 24 hours of run time) as
it is refitting the modern calibration and reconstruction models 10
times.

``` r
valid_run <- run_valid(modern_elevation = NFLD_elevation, 
                       modern_species = NFLD_modern_species,
                       n_folds = 10)
```
