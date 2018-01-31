01\_explore-libraries\_jenny.R
================
Emily
Wed Jan 31 14:08:23 2018

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.3/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
library(fs)
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.3/Resources/library

Installed packages

``` r
library(tidyverse)
```

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 211

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##                                                          LibPath
    ##                                                            <chr>
    ## 1 /Library/Frameworks/R.framework/Versions/3.3/Resources/library
    ## 2 /Library/Frameworks/R.framework/Versions/3.3/Resources/library
    ## 3 /Library/Frameworks/R.framework/Versions/3.3/Resources/library
    ## # ... with 2 more variables: Priority <chr>, n <int>

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n       prop
    ##              <chr> <int>      <dbl>
    ## 1               no    95 0.45023697
    ## 2              yes   104 0.49289100
    ## 3             <NA>    12 0.05687204

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   Built     n      prop
    ##   <chr> <int>     <dbl>
    ## 1 3.3.0    63 0.2985782
    ## 2 3.3.2   106 0.5023697
    ## 3 3.3.3    42 0.1990521

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

    ##   [1] "abettor"         "acepack"         "assertthat"     
    ##   [4] "backports"       "base64enc"       "BH"             
    ##   [7] "bindr"           "bindrcpp"        "bit"            
    ##  [10] "bit64"           "bitops"          "blob"           
    ##  [13] "brew"            "broom"           "car"            
    ##  [16] "caTools"         "cellranger"      "checkmate"      
    ##  [19] "classInt"        "cli"             "clipr"          
    ##  [22] "clisymbols"      "colorspace"      "commonmark"     
    ##  [25] "countrycode"     "covr"            "crayon"         
    ##  [28] "crosstalk"       "crunch"          "crunchtabs"     
    ##  [31] "curl"            "data.table"      "DBI"            
    ##  [34] "desc"            "devtools"        "dichromat"      
    ##  [37] "digest"          "dplyr"           "e1071"          
    ##  [40] "enc"             "evaluate"        "forcats"        
    ##  [43] "formattable"     "Formula"         "fs"             
    ##  [46] "gdata"           "gdtools"         "ggplot2"        
    ##  [49] "ggrepel"         "gh"              "git2r"          
    ##  [52] "glue"            "googlesheets"    "gridExtra"      
    ##  [55] "gtable"          "gtools"          "haven"          
    ##  [58] "here"            "hexbin"          "highr"          
    ##  [61] "Hmisc"           "hms"             "htmlTable"      
    ##  [64] "htmltools"       "htmlwidgets"     "httpcache"      
    ##  [67] "httptest"        "httpuv"          "httr"           
    ##  [70] "ini"             "jsonlite"        "knitr"          
    ##  [73] "labeling"        "labelled"        "latticeExtra"   
    ##  [76] "lazyeval"        "lme4"            "lubridate"      
    ##  [79] "magrittr"        "mailR"           "markdown"       
    ##  [82] "MatrixModels"    "memisc"          "memoise"        
    ##  [85] "mime"            "miniUI"          "minqa"          
    ##  [88] "mnormt"          "modelr"          "mschart"        
    ##  [91] "munsell"         "nloptr"          "officer"        
    ##  [94] "openssl"         "openxlsx"        "pbkrtest"       
    ##  [97] "pkgconfig"       "plogr"           "plotly"         
    ## [100] "plyr"            "png"             "praise"         
    ## [103] "pryr"            "psych"           "purrr"          
    ## [106] "quantreg"        "questionr"       "R.cache"        
    ## [109] "R.methodsS3"     "R.oo"            "R.utils"        
    ## [112] "R6"              "RColorBrewer"    "Rcpp"           
    ## [115] "RcppEigen"       "RCurl"           "readr"          
    ## [118] "readstata13"     "readxl"          "rematch"        
    ## [121] "rematch2"        "ReporteRs"       "ReporteRsjars"  
    ## [124] "repurrrsive"     "reshape"         "reshape2"       
    ## [127] "rex"             "rJava"           "rjson"          
    ## [130] "RJSONIO"         "rlang"           "rlist"          
    ## [133] "rmarkdown"       "rowr"            "roxygen2"       
    ## [136] "rprojroot"       "RSQLite"         "rstudioapi"     
    ## [139] "rvest"           "rvg"             "scales"         
    ## [142] "SDMTools"        "selectr"         "sendmailR"      
    ## [145] "sf"              "shiny"           "shinycssloaders"
    ## [148] "slackr"          "slam"            "sourcetools"    
    ## [151] "SparseM"         "sss"             "sticky"         
    ## [154] "stringi"         "stringr"         "styler"         
    ## [157] "survey"          "tau"             "testthat"       
    ## [160] "textcat"         "tibble"          "tidyr"          
    ## [163] "tidyselect"      "tidyverse"       "translateR"     
    ## [166] "translations"    "twitteR"         "udunits2"       
    ## [169] "units"           "usethis"         "uuid"           
    ## [172] "viridis"         "viridisLite"     "whisker"        
    ## [175] "withr"           "writexl"         "xlsx"           
    ## [178] "xlsxjars"        "XML"             "xml2"           
    ## [181] "xtable"          "yaml"            "zip"

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
    ##   github     n      prop
    ##    <lgl> <int>     <dbl>
    ## 1  FALSE   100 0.4739336
    ## 2   TRUE   111 0.5260664
