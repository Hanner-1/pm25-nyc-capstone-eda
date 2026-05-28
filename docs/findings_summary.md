# Findings Summary

## Purpose
This document summarizes the main findings from the PM2.5 NYC Air Quality Capstone exploratory data analysis.

The findings are written for a public-facing portfolio audience and should be reviewed against the final notebook before publication.

## Project Focus
This project explored PM2.5 air quality data in New York City using Python and Google Colab.

The analysis focused on:
- PM2.5 concentration patterns
- Monitoring-site differences
- Missing-value handling
- Threshold-based interpretation
- Short-term concentration spikes
- Public health communication

## Main Findings
This analysis examined hourly PM2.5 measurements from New York City monitoring stations from May 1 to May 19, 2026. The active capstone dataset included a controlled missing-value case so that missing-value detection, imputation, and clean-data verification could be demonstrated.

- After imputation, the dataset contained **5,608 records** with **0 missing values** in the analysis column, `Value_Imputed`. Before imputation, there were **109 intentionally missing PM2.5 values**, representing approximately **1.94%** of the dataset.

- Overall PM2.5 levels were moderate during the study period. The average PM2.5 value was approximately **6.69 µg/m³**, and the median was approximately **5.53 µg/m³**. The minimum observed PM2.5 value was **2.19 µg/m³**, and the maximum observed value was **32.10 µg/m³**.

- The highest PM2.5 reading occurred at **Manhattan Bridge** on **May 10, 2026 at 16:00**, with a value of **32.10 µg/m³**. The lowest PM2.5 reading was **2.19 µg/m³**, recorded at **Broadway/35th St** on **May 7, 2026 at 06:00**.

- No hourly readings exceeded **35 µg/m³**, and no daily location-level averages exceeded the **35 µg/m³ 24-hour standard**. However, there were **841 hourly records at or above 10 µg/m³**, and **4 hour-to-hour increases of 10 µg/m³ or more**, which suggests that short-term elevated PM2.5 periods occurred even though the 24-hour standard was not exceeded.

- Location-level results showed that **Midtown West** had the highest average PM2.5 value, followed by locations such as **Hamilton Bridge**, **Broadway/35th St**, **Cross Bronx Expy**, and **Hunt's Point**. However, Midtown West had only **40 records**, so it should be interpreted as a low-record comparison station rather than as the strongest location-level conclusion.

- Time-based analysis showed that **May 10, 2026** had the highest daily average PM2.5 level during the analysis period. The hourly analysis showed modest variation by hour of day, with the highest average hour around **15:00** and the lowest average hour around **6:00**.

- The boxplots showed that the PM2.5 distribution was right-skewed. Most values were concentrated in the lower range, while several higher readings appeared as upper outliers. These outliers support the finding that short-term elevated readings occurred during the study period.

## Public Health Interpretation
PM2.5 is a fine particulate pollutant associated with respiratory and cardiovascular health concerns.

This project uses PM2.5 data to practice responsible public health data communication.

The findings should be interpreted as exploratory and educational, not as a regulatory assessment or causal health study.

## Recommendations
Based on the analysis workflow, recommended next steps include:

1. Verify the final notebook outputs before publishing.
2. Add exact summary values and chart references from the final notebook.
3. Review monitoring-site coverage before making geographic claims.
4. Compare PM2.5 patterns with weather or event data if available.
5. Keep public health conclusions cautious and evidence-based.
6. Use the project as a portfolio example of environmental data analysis.

## Portfolio Value
This project demonstrates skills that are relevant to data analyst and public health data roles, including:

- Python-based exploratory data analysis
- data cleaning and missing-value strategy
- working with environmental datasets
- merging data with metadata