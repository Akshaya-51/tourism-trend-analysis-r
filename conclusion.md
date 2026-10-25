# Conclusion

**Case study:** Tourism Trend Analysis and Tourist Arrival Prediction Using R
**Course:** Introduction to Data Science using R

## 1. What we did

We analysed World Bank data on international tourist arrivals (indicator
`ST.INT.ARVL`, World Development Indicators), downloaded in R with the `WDI`
package for all countries and regions from 1960 to 2023.

- The original download had **17,024 rows and 5 columns** (country, iso2c, iso3c,
  year, arrivals). Most country-year combinations had no value.
- After removing missing arrivals, **5,806 rows** remained. These still included
  regional totals such as "World" and income groups.
- After removing those totals so that only countries were compared, **4,683 rows for
  203 countries** remained, covering **1995 to 2020**. The data has no values after
  2020.
- We explored the trends of the five largest destinations in 2019 (France, the
  United States, China, Spain and Mexico) and India, then built three models from the
  course: linear regression, a generalized additive model (GAM) and a random forest.
- The models predict a country's log arrivals from last year's log arrivals and the
  year. Years up to 2019 were used, because the 2020 pandemic is an exceptional shock.
  The split was by time: **training on 1995 to 2014 (3,381 rows)** and **testing on
  2015 to 2019 (936 rows)**.

## 2. What the plots show

1. **Arrivals over time (line chart).** France is highest, at about 218 million
   arrivals in 2019, followed by the United States (about 165 million), China
   (about 163 million), Spain (about 126 million) and Mexico (about 97 million).
   India is much smaller, at about 18 million. Most countries rise steadily until
   2019 and then fall sharply in 2020. The points show where data exists: France has
   no values for 1998 to 2003. The United States jumps in 2006 (from about 72 to
   about 183 million), which looks like a change in how arrivals are counted and not
   a real surge.

2. **Log-scale chart.** On a log scale a steeper line means faster percentage growth,
   so small and large countries can be compared fairly. India rises more steeply than
   most countries, including a step around 2014, although its absolute arrivals
   remain small.

3. **Year-on-year growth (bar charts).** In normal years growth is small, mostly a
   few percent a year, and France is very stable. Two large spikes stand out, the
   United States in 2006 (about +150%) and India in 2014 (about +88%), and these look
   like data breaks and not real growth. Every country shows a large fall in 2020.
   Each panel has its own vertical scale, so the shapes should be compared rather
   than the bar heights.

4. **Arrivals indexed to 1995 = 100.** Indexing compares each country with its own
   1995 level. India reaches about eight times its 1995 level, but much of this comes
   from the 2014 jump. China is about 3.5 times its 1995 level, Spain and the United
   States about twice, and Mexico stays near its starting level. France is missing
   because it has no value for 1995.

5. **Effect of the pandemic (2019 vs 2020).** Arrivals fell by about 46% in France,
   47.5% in Mexico, 71% in Spain, 73% in the United States and 81% in China. India has
   no 2020 value in the World Bank data, so it has no bar.

6. **Actual vs predicted arrivals.** Points close to the red line are accurate
   predictions. Linear regression and the GAM follow the line closely across the
   whole range. The random forest flattens at the largest values and overpredicts the
   smallest countries.

## 3. Model results (test years 2015 to 2019)

| Model | MAE | RMSE | R-squared |
|---|---|---|---|
| Linear regression | 0.54 | 1.29 | 0.998 |
| GAM | 1.18 | 3.05 | 0.988 |
| Random forest | 3.89 | 15.5 | 0.679 |

MAE and RMSE are in millions of tourists. **Linear regression scored best on all
three measures**, followed by the GAM, with the random forest weakest.

## 4. What we observed

- **Tourism is highly concentrated.** A few countries receive far more visitors than
  the rest, and India's arrivals are small in absolute terms even though they grew
  fast relative to its own 1995 level.
- **Last year's arrivals explains almost everything.** In the linear model its
  coefficient is 0.989, and the year term is not significant (p = 0.76). In the random
  forest, last year's arrivals has an importance of about 14,125 against about 223 for
  the year. In the GAM, the smooth term for last year's arrivals is almost a straight
  line (effective degrees of freedom about 1.07). So a country's arrivals in one year
  are roughly last year's arrivals multiplied by a growth factor.
- **A more complex model was not better here.** The simplest model, linear regression,
  was the most accurate. The random forest is weakest because tree models cannot
  predict outside the range of values they were trained on, and the test years contain
  larger values than the training years.
- **The pandemic was a major shock.** Arrivals fell by roughly half to four-fifths in
  the countries studied. This is why 2020 was left out of the models.
- **Some series contain breaks.** India (2014) and the United States (2006) show jumps
  that look like changes in counting, so growth comparisons across countries should be
  made with care.

## 5. Limitations

- The models predict one year ahead using the actual previous year, which is easier
  than forecasting several years ahead. A simple benchmark of repeating last year's
  value would also be expected to score well.
- Some series have missing years and breaks, and countries may count arrivals in
  different ways, so comparisons are not perfect.
- R-squared is dominated by the largest countries, so MAE and RMSE should be read
  alongside it.
- The data is annual, so seasonality cannot be studied. The pandemic years are excluded
  from the models, and the downloaded data ends in 2020.
- Country was not used as a predictor, because `randomForest` cannot handle a factor
  with more than 53 levels.

## 6. Recommended actions

- Use last year's arrivals as the starting point for short-term demand planning.
- Keep a margin for shocks such as the pandemic, which no model based on past arrivals
  can foresee.
- Check how arrivals are counted before comparing countries or acting on sudden jumps.

## 7. Future scope

- Add predictors such as GDP, exchange rates and tourism receipts.
- Model the pandemic and recovery period separately.
- Forecast several years ahead instead of one.
- Add monthly data to study seasonality.
- Build an interactive dashboard for exploring the results.

## Final conclusion

Historical arrivals are a strong guide to the next year's arrivals. Using World Bank data
for 203 countries, a simple linear regression on last year's arrivals predicted the test
years better than a GAM or a random forest (R-squared 0.998, RMSE about 1.3 million
tourists). Tourism is concentrated in a few large destinations, grew steadily until 2019,
and was hit very hard in 2020. The main caution is data quality: breaks in some series
and the missing 2020 values mean results should be read with care.
