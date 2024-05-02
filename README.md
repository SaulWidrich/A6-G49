# periodicity
Repository for work on analyses of periodicity in Canadian communicable disease incidence

## Getting the Data

This project will use data that can be obtained using this command in R.

```
api_hook = iidda.api::ops_staging
canmod_cdi_api = api_hook$filter(
    resource_type = "CANMOD CDI"
  , iso_3166 = "CA"
)
```

Running this command in R requires the [iidda.api](https://canmod.github.io/iidda-tools/iidda.api) package, which you can get using this command in R.

```
install.packages("iidda.api"
  , repos = c(
      "https://canmod.r-universe.dev"
    , "https://cran.r-project.org"
  )
)
```



## Project Goals

0. Be able to create a periodogram for any dataset with un-evenly sampled incidence data (SW fully understands this)
1. Develop a statistic for measuring the 'seasonality' of a disease (SW 90% understands this)
2. 
