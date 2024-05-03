# periodicity
Repository for work on analyses of periodicity in Canadian communicable disease incidence

Quiz: Create a section in this document with links to the various references that we have talked about so far (at the bottom)

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

You can explore these `data` with `dplyr`. For example, this will get you all weekly measles data in Ontario (something to work with anyways).

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

Steve and Saul will both collaborate to produce this project notebook.  This is where we put our notes on what we have learned and what we are thinking.

### Periodograms

Be able to create a periodogram for any dataset with un-evenly sampled incidence data (Steve fully understands this). This is a warm-up to the research that will involve getting comfortable with manipulating data and using the `lomb`  package and maybe others that we encounter.

*Quiz*: Use `dplyr` to update the `measles_wk_on` example above to contain a daily incidence rate.
```
measles_wk_on <- measles_wk_on %>% 
        mutate(daily_rate = 10^5 * cases_this_period / (days_this_period * population))
```
Hint:

$$
\text{daily-rate} = 10^5 \times \frac{\text{cases-this-period}}{ \text{days-this-period} \times \text{population}}
$$

*Quiz*: Use `dplyr` and something like the `difftime` function to produce a column measuring the number of years since the first observation in the dataset.

```
measles_wk_on <- measles_wk_on %>%
  mutate(years_since_first_observation = as.numeric((period_start_date - min(period_start_date)) / (365.25)))
```

*Quiz*: Make a Lomb-Scargle periodogram of the 'number of years' variable as the time variable and the incidence rate as the series variable (or whatever the `lomb::lsp` function calls it.

```
periodogram <- lomb::lsp(measles_wk_on$years_since_first_observation, measles_wk_on$daily_rate, type = 'period', plot = "True")

```
### Seasonality Statistics

Develop statistics for measuring the 'seasonality' of a disease (Steve 90% understands this).

We have tried dividing the sum of the power 'near' (defining what this means is part of the problem) a period of 1-year by the sum of the power over a 'broader' (defining what this means is part of the problem) range of periods.  This is a good start because this statistic will tend to be higher when there is relatively more power around periods of one year.

In general we are considering statistics of the following form.

$$
\text{seasonality} = \frac{\sum_{i \in \Omega} y_i }{\sum_i y_i}
$$

Where $y_i$ is the thing on the y-axis of a periodogram associated with period given by $x_i$, and where $\Omega$ is the set of periods 'near' periods of interest (i.e. one year).

Issues:

1. When comparing diseases the periodograms might have different numbers of frequencies sampled and therefore the size of $\Omega$ relative to the full range of frequencies might be different. To make things more comparable across diseases we might want to consider using averages and not sums in the above formula (Steve is currently unsure).
2. Do we want to put periods in $\Omega$ that are near 1, 2, 3, ..., years or just near 1 year.
3. Do we want to measure 'nearness' as any frequency +/- some tolerance, as the one frequency that is closest to the target period (e.g. 1 year). One thing I think is for sure is that we want to be measuring nearness in frequencies and not periods because frequencies are evenly spaced whereas periods are not (Quiz: What does this mean?  Hint: look at the differences between adjacent periods versus adjacent frequencies in Lomb-Scargle output).
4. Do we want our seasonality statistic to be more sensitive to periods under a year, over a year, or something balanced? For example, which is less seasonal -- a disease that has lots of power less than a year but not much greater than a year, or vice versa? This issue is related to the fact of uneven sampling of periods and even sampling of frequencies.
5. Do we want to normalize in some way over diseases so that, say, the most seasonal disease is 100 or somthing and the maximally unseasonal disease is 0?


Goal: Design and conduct a simulation study that we can do that will rank a set of diseases by seasonality differently depending on different methodological choices listed in these issues. The outcome of the experiment can be the correlation of the ranks with some baseline method. If the correlations are high it says that the methodological choices do not matter.  If the correlations are low it says that they do matter.  Trying to be clearer ... for every disease we will compute a set of `n` seasonality statistics.  We get these different seasonality statistics by making different methodological choices.  Then we calculate the correlations between the statistics (Hint: `cor` function in R). See [this](https://en.wikipedia.org/wiki/Correlation_coefficient) for background on correlation coefficients.


### Phase Estimation

Develop code for estimating where the peaks and troughs are within a cycle of a particular period.


### Time-Varying Periodograms

Develop code for estimating 

## References: 

* R for data science: https://r4ds.had.co.nz
* Background on Mathematical Epidemiology: https://davidearn.mcmaster.ca/opportunities
