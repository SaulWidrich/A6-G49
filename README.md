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
"grDevices", "lomb"))
```

## Getting the Data

This project will use data that can be obtained using this command in R.

```
api_hook = iidda.api::ops_staging
cdi = api_hook$filter(
    resource_type = "CANMOD CDI"
  , iso_3166 = "CA"
)
```

You can explore these data with `dplyr`. For example, this will get you all weekly measles data in Ontario (something to work with anyways).

```
measles_wk_on = (cdi
  |> filter(disease == "measles", iso_3166_2 == "CA-ON", time_scale == "wk")
)
```

## Project Goals

0. Periodograms
1. Seasonality Statistics
2. Phase Estimationperiod
3. Time-Varying Periodograms

## Shared Notebook

### Periodograms

Be able to create a periodogram for any dataset with un-evenly sampled incidence data (Steve fully understands this). This is a warm-up to the research that will involve getting comfortable with manipulating data and using the `lomb`  package and maybe others that we encounter.

Quiz: Use `dplyr` to update the `measles_wk_on` example above to contain a daily incidence rate.

Hint:
$$
daily\_rate = 10^5 \times \frac{\mathtt{cases\_this\_period}}{ \mathtt{days\_this\_period} \times \mathtt{population}}
$$

### Seasonality Statistics

Develop statistics for measuring the 'seasonality' of a disease (SW 90% understands this).


### Phase Estimation

Develop code for estimating where the peaks and troughs are within a cycle of a particular period.


### Time-Varying Periodograms

Develop code for estimating 

