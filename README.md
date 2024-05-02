# periodicity
Repository for work on analyses of periodicity in Canadian communicable disease incidence

## Dependencies

This project requires installing several R dependencices.

The first one to get is the [iidda.api](https://canmod.github.io/iidda-tools/iidda.api) package, which you can get using this command in R.

```
install.packages("iidda.api"
  , repos = c(
      "https://canmod.r-universe.dev"
    , "https://cran.r-project.org"
  )
)
```

Here are some others that I also recommend (but we might need more / fewer).  They can be installed in this way.

```
install.packages(c("dplyr", "lubridate", 
"broom", "Hmisc", "readr", "jsonlite", "ggplot2", "patchwork", 
"grDevices", "gt", "lomb"))
```

## Getting the Data

This project will use data that can be obtained using this command in R.

```
api_hook = iidda.api::ops_staging
canmod_cdi_api = api_hook$filter(
    resource_type = "CANMOD CDI"
  , iso_3166 = "CA"
)
```




## Project Goals

0. Periodograms : Be able to create a periodogram for any dataset with un-evenly sampled incidence data (SW fully understands this)
1. Seasonality Statistics : Develop statistics for measuring the 'seasonality' of a disease (SW 90% understands this)
2. Develop code for estimating 
