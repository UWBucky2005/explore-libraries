01\_explore-libraries\_jenny.R
================
TravisWolter1
Wed Jan 31 14:05:39 2018

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.4/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
library(fs)
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.4/Resources/library

Installed packages

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.4
    ## ✔ tidyr   0.8.0     ✔ stringr 1.2.0
    ## ✔ readr   1.1.1     ✔ forcats 0.2.0

    ## ── Conflicts ────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 177

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##   LibPath                                                 Priority       n
    ##   <chr>                                                   <chr>      <int>
    ## 1 /Library/Frameworks/R.framework/Versions/3.4/Resources… base          14
    ## 2 /Library/Frameworks/R.framework/Versions/3.4/Resources… recommend…    15
    ## 3 /Library/Frameworks/R.framework/Versions/3.4/Resources… <NA>         148

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                  81 0.458 
    ## 2 yes                 89 0.503 
    ## 3 <NA>                 7 0.0395

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 4 x 3
    ##   Built     n  prop
    ##   <chr> <int> <dbl>
    ## 1 3.4.0    61 0.345
    ## 2 3.4.1    18 0.102
    ## 3 3.4.2    28 0.158
    ## 4 3.4.3    70 0.395

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##   [1] "abind"                "acepack"              "assertthat"          
    ##   [4] "backports"            "base64enc"            "BH"                  
    ##   [7] "bindr"                "bindrcpp"             "bitops"              
    ##  [10] "broom"                "callr"                "caTools"             
    ##  [13] "cellranger"           "checkmate"            "cli"                 
    ##  [16] "clipr"                "clisymbols"           "colorspace"          
    ##  [19] "corrplot"             "covr"                 "crayon"              
    ##  [22] "curl"                 "data.table"           "DBI"                 
    ##  [25] "dbplyr"               "debugme"              "desc"                
    ##  [28] "devtools"             "dichromat"            "digest"              
    ##  [31] "DMwR"                 "dplyr"                "enc"                 
    ##  [34] "evaluate"             "expsmooth"            "fma"                 
    ##  [37] "forcats"              "forecast"             "Formula"             
    ##  [40] "fpp"                  "fracdiff"             "fs"                  
    ##  [43] "gdata"                "gdtools"              "ggplot2"             
    ##  [46] "ggplot2movies"        "gh"                   "git2r"               
    ##  [49] "glue"                 "gplots"               "gridExtra"           
    ##  [52] "gtable"               "gtools"               "haven"               
    ##  [55] "hexbin"               "highr"                "Hmisc"               
    ##  [58] "hms"                  "htmlTable"            "htmltools"           
    ##  [61] "htmlwidgets"          "httr"                 "ini"                 
    ##  [64] "jsonlite"             "knitr"                "labeling"            
    ##  [67] "latticeExtra"         "lazyeval"             "lmtest"              
    ##  [70] "lubridate"            "magrittr"             "mapproj"             
    ##  [73] "maps"                 "maptools"             "markdown"            
    ##  [76] "MatrixModels"         "memoise"              "mime"                
    ##  [79] "mnormt"               "modelr"               "multcomp"            
    ##  [82] "munsell"              "mvtnorm"              "openssl"             
    ##  [85] "padr"                 "PerformanceAnalytics" "pillar"              
    ##  [88] "pkgconfig"            "plogr"                "plyr"                
    ##  [91] "praise"               "psych"                "purrr"               
    ##  [94] "quadprog"             "Quandl"               "quantmod"            
    ##  [97] "quantreg"             "R6"                   "RColorBrewer"        
    ## [100] "Rcpp"                 "RcppArmadillo"        "readr"               
    ## [103] "readxl"               "rematch"              "rematch2"            
    ## [106] "reprex"               "reshape2"             "rex"                 
    ## [109] "rJava"                "rlang"                "rmarkdown"           
    ## [112] "ROCR"                 "rprojroot"            "rstudioapi"          
    ## [115] "rvest"                "sandwich"             "scales"              
    ## [118] "selectr"              "sp"                   "SparseM"             
    ## [121] "stringi"              "stringr"              "styler"              
    ## [124] "svglite"              "testthat"             "TH.data"             
    ## [127] "tibble"               "tidyquant"            "tidyr"               
    ## [130] "tidyselect"           "tidyverse"            "timeDate"            
    ## [133] "timeSeries"           "timetk"               "translations"        
    ## [136] "tseries"              "TTR"                  "usethis"             
    ## [139] "utf8"                 "viridis"              "viridisLite"         
    ## [142] "whisker"              "withr"                "XLConnect"           
    ## [145] "XLConnectJars"        "xml2"                 "xts"                 
    ## [148] "yaml"                 "zoo"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 F         87 0.492
    ## 2 T         90 0.508
