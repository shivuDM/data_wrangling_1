data manipulation with dplyr
================

``` r
library (tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.0      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

options(tibble.print_min = 3)

``` r
litters_data = read_csv("data_import_examples/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_data = janitor::clean_names(litters_data)
```

``` r
pups_data = read_csv("data_import_examples/FAS_pups.csv")
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_data = janitor::clean_names(pups_data)
```

\#select

``` r
select(litters_data, group, litter_number, gd0_weight, pups_born_alive)
```

    ## # A tibble: 49 × 4
    ##    group litter_number   gd0_weight pups_born_alive
    ##    <chr> <chr>                <dbl>           <dbl>
    ##  1 Con7  #85                   19.7               3
    ##  2 Con7  #1/2/95/2             27                 8
    ##  3 Con7  #5/5/3/83/3-3         26                 6
    ##  4 Con7  #5/4/2/95/2           28.5               5
    ##  5 Con7  #4/2/95/3-3           NA                 6
    ##  6 Con7  #2/2/95/3-2           NA                 6
    ##  7 Con7  #1/5/3/83/3-3/2       NA                 9
    ##  8 Con8  #3/83/3-3             NA                 9
    ##  9 Con8  #2/95/3               NA                 8
    ## 10 Con8  #3/5/2/2/95           28.5               8
    ## # … with 39 more rows

``` r
select(litters_data, group:gd_of_birth)
```

    ## # A tibble: 49 × 5
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>
    ##  1 Con7  #85                   19.7        34.7          20
    ##  2 Con7  #1/2/95/2             27          42            19
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19
    ##  5 Con7  #4/2/95/3-3           NA          NA            20
    ##  6 Con7  #2/2/95/3-2           NA          NA            20
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20
    ##  8 Con8  #3/83/3-3             NA          NA            20
    ##  9 Con8  #2/95/3               NA          NA            20
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20
    ## # … with 39 more rows

``` r
select(litters_data, -pups_survive)
```

    ## # A tibble: 49 × 7
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_…¹ pups_…²
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>        <dbl>   <dbl>
    ##  1 Con7  #85                   19.7        34.7          20            3       4
    ##  2 Con7  #1/2/95/2             27          42            19            8       0
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19            6       0
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19            5       1
    ##  5 Con7  #4/2/95/3-3           NA          NA            20            6       0
    ##  6 Con7  #2/2/95/3-2           NA          NA            20            6       0
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20            9       0
    ##  8 Con8  #3/83/3-3             NA          NA            20            9       1
    ##  9 Con8  #2/95/3               NA          NA            20            8       0
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20            8       0
    ## # … with 39 more rows, and abbreviated variable names ¹​pups_born_alive,
    ## #   ²​pups_dead_birth

\#rename

``` r
select(litters_data, GROUP = group, LiTtEr_NuMbEr = litter_number)
```

    ## # A tibble: 49 × 2
    ##    GROUP LiTtEr_NuMbEr  
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # … with 39 more rows

\#use rename instead of select

``` r
rename(litters_data, GROUP = group, LiTtEr_NuMbEr = litter_number)
```

    ## # A tibble: 49 × 8
    ##    GROUP LiTtEr_NuMbEr   gd0_weight gd18_weight gd_of_…¹ pups_…² pups_…³ pups_…⁴
    ##    <chr> <chr>                <dbl>       <dbl>    <dbl>   <dbl>   <dbl>   <dbl>
    ##  1 Con7  #85                   19.7        34.7       20       3       4       3
    ##  2 Con7  #1/2/95/2             27          42         19       8       0       7
    ##  3 Con7  #5/5/3/83/3-3         26          41.4       19       6       0       5
    ##  4 Con7  #5/4/2/95/2           28.5        44.1       19       5       1       4
    ##  5 Con7  #4/2/95/3-3           NA          NA         20       6       0       6
    ##  6 Con7  #2/2/95/3-2           NA          NA         20       6       0       4
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA         20       9       0       9
    ##  8 Con8  #3/83/3-3             NA          NA         20       9       1       8
    ##  9 Con8  #2/95/3               NA          NA         20       8       0       8
    ## 10 Con8  #3/5/2/2/95           28.5        NA         20       8       0       8
    ## # … with 39 more rows, and abbreviated variable names ¹​gd_of_birth,
    ## #   ²​pups_born_alive, ³​pups_dead_birth, ⁴​pups_survive
