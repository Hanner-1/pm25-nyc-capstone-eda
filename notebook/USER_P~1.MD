# Python Analysis Capstone Notebook

Converted from the original Google Colab `.ipynb` notebook into a clean Markdown source for NotebookLM.

This version preserves explanatory markdown and code cells, while omitting most raw cell outputs to keep the source readable.


## Conversion Summary

- Source file: `User_PythonAnalysis_Capstone.ipynb`
- Total preserved cells: 69
- Markdown cells: 39
- Code cells: 30
- Outputs: omitted for NotebookLM readability


---

# **Capstone**: EDA for the PM2.5 in NYC, May-2026


---

# **0. Introduction**


---

## **Context**
PM2.5 refers to fine particulate matter that is 2.5 micrometers or smaller in diameter and is considered one of the most harmful air pollutants because it can travel deep into the lungs and affect heart and respiratory health. In NYC, long-term exposure to PM2.5 is linked to an estimated 2,000 excess deaths from lung and heart disease each year, while short-term exposure can contribute to asthma-related emergency department visits and other health threats; in this project, PM2.5 is measured in µg/m³ to evaluate air quality patterns across monitoring locations. (1).


(1) https://a816-dohbesp.nyc.gov/IndicatorPublic/data-features/realtime-air-quality/


---

## N**ew York City PM₂.₅ Air Quality Tracking Framework**

| Feature | 1‑Hour Framework | 24‑Hour Regulatory Standard | Annual / Baseline Metrics |
|---------|------------------|-----------------------------|----------------------------|
| **Legal Status** | No statutory limit (no legal penalty or fines apply) | Legally binding federal threshold | — |
| **Threshold Value** | N/A (tracks live raw metrics only) | **35 µg/m³** | — |
| **Pristine Reference** | — | — | 2.5 µg/m³ (pristine background air; zero health risk) |
| **Clinical Threshold** | **10 µg/m³** incremental spike triggers measurable increases in heart/lung hospital visits | — | — |
| **Primary Purpose** | Immediate public alerts and short‑term visibility | Federal regulatory compliance and regional state planning | — |
| **Data Calculation** | NowCast Algorithm (weights the last 12 hours of rolling data) | Strict midnight‑to‑midnight calendar‑day blocks | — |
| **Compliance Test** | N/A | 3‑year average of annual 98th percentile values | — |
| **Reporting Tool** | Live AirNow AQI maps and phone alerts | NYSDEC annual regulatory reports | — |
| **Framework Inception** | — | — | 2006 (24‑hour limit lowered to 35 µg/m³) |
| **AQI Formula Update** | — | — | 2012 (NowCast algorithm standardized for modern maps) |
| **Recent Policy Revision** | — | — | 2024 (annual baseline lowered to 9 µg/m³) [1, 2] |


The project will therefore use two interpretation levels:
1. **Primary threshold**: 24-hour PM2.5 standard of 35 micrograms per cubic meter, evaluated using daily station averages.
2. **Secondary clinical spike flag**: short-term PM2.5 spikes that may deserve attention even when the 24-hour standard is not exceeded.


---

## **Problem Statement**

The project investigates PM2.5 air quality patterns in NYC during May 1-19, 2026.

The key problem is that overall air quality can appear acceptable while some places and hours may still experience short-term PM2.5 increases. The analysis should identify whether PM2.5 levels remained below relevant thresholds, where the highest peaks occurred, where the lowest levels appeared, and whether short-term spikes deserve attention.


---

## **Project Objective**
Analyze hourly PM2.5 measurements from NYC street-level monitors for May 1-19, 2026, using exploratory data analysis to identify overall patterns, missing values, station-level differences, peak values, low values, threshold status, and practical recommendations.


---

## **Analysis Questions**

1. What is the overall PM2.5 pattern in NYC during May 1-19, 2026?
2. Are observed PM2.5 values generally below the 24-hour standard of 35 micrograms per cubic meter?
3. Which monitoring locations recorded the highest PM2.5 peaks?
4. Which monitoring locations recorded the lowest PM2.5 levels?
5. How does PM2.5 vary by time and station location?
6. Are there missing values or data-quality issues that affect interpretation?
7. Were there short-term PM2.5 spikes that deserve attention even if the 24-hour standard was not exceeded?


---

## **Dataset Description**
The dataset contains hourly PM2.5 air quality measurements from NYC monitoring stations for May 1–19, 2026, with each record including a site ID, timestamp, and PM2.5 value in µg/m³. A separate station metadata file provides location names and geographic coordinates, allowing the analysis to compare PM2.5 patterns across monitoring locations.

### **Main Dataset Fields**:

| Field | Meaning |
|-------|---------|
| **[ID](ca://s?q=Explain_the_ID_field_in_this_dataset)** | Record identifier. |
| **[SiteID](ca://s?q=What_is_the_SiteID_field_used_for)** | Monitoring station identifier. |
| **[ObservationTimeUTC](ca://s?q=Explain_ObservationTimeUTC_timestamp_format)** | Timestamp for the start of the hourly measurement period. |
| **[Value](ca://s?q=What_does_the_PM2.5_Value_field_represent)** | PM₂.₅ concentration in micrograms per cubic meter. |


### **Station Metadata Fields**:

| Field | Meaning |
|-------|---------|
| **[ID](ca://s?q=Explain_the_dataset_ID_field)** | Record identifier. |
| **[SiteID](ca://s?q=Describe_the_SiteID_monitoring_station_field)** | Monitoring station identifier. |
| **[ObservationTimeUTC](ca://s?q=Explain_ObservationTimeUTC_timestamp_format)** | Timestamp for the start of the hourly measurement period. |
| **[Value](ca://s?q=Explain_the_PM2.5_Value_field)** | PM₂.₅ concentration in micrograms per cubic meter. |

### **Data Citation**:
New York City Department of Health and Mental Hygiene and Queens College. New York City Community Air Survey. Real-time PM2.5 Monitor Data from 05-01-2026 to 05-19-2026. https://a816-dohbesp.nyc.gov/IndicatorPublic/data-features/realtime-air-quality/.

Date downloaded: 05/19/26.

Note: Data are preliminary and subject to change. External factors can sometimes affect monitor functioning and make data temporarily unavailable.


---

# **1. Load Libraries**


---

## Code Cell 1

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# EDA display settings: show all DataFrame columns and control console output width in Colab
pd.set_option("display.max_columns", None)
pd.set_option("display.width", 120)
```


---

# **2. Load Source Data**


---

The Capstone reference file had timestamp formatting issues after CSV export, so the original clean PM2.5 file will used as the timestamp-safe source of truth. The Capstone reference file will used only to identify which PM2.5 `Value` records were intentionally set to missing for imputation practice.


---

## Code Cell 2

```python
# ============================================================
# Capstone Project: EDA for PM2.5 in NYC, May 2026
# Data Loading Strategy:
# 1. Load original clean dataset as timestamp-safe source of truth.
# 2. Load capstone dataset only to identify missing Value cells.
# 3. Transfer missing-value pattern by ID.
# 4. Impute missing values.
# 5. Perform EDA.
# ============================================================

SOURCE_PM25_URL = "https://raw.githubusercontent.com/Hanner-1/pm25-nyc-capstone-eda/refs/heads/main/PM2.5_may_2026.csv"
CAPSTONE_PM25_URL = "https://raw.githubusercontent.com/Hanner-1/pm25-nyc-capstone-eda/refs/heads/main/PM2.5_may_2026_Capstone.csv"
SITE_INFO_URL = "https://raw.githubusercontent.com/Hanner-1/pm25-nyc-capstone-eda/refs/heads/main/site_info.csv"

source_df = pd.read_csv(SOURCE_PM25_URL)
capstone_missing_ref_df = pd.read_csv(CAPSTONE_PM25_URL)
site_info_df = pd.read_csv(SITE_INFO_URL)

print("Source PM2.5 data loaded:", source_df.shape)
print("Capstone missing-reference data loaded:", capstone_missing_ref_df.shape)
print("Site info data loaded:", site_info_df.shape)
```


---

# **3. Inspect Raw Data**


---

## Code Cell 3

```python
print("Source dataset preview:")
display(source_df.head())

print("Capstone missing-reference preview:")
display(capstone_missing_ref_df.head())

print("Site info preview:")
display(site_info_df.head())
```


---

## Code Cell 4

```python
print("Source columns and data types:")
print(source_df.dtypes)

print("\nCapstone reference columns and data types:")
print(capstone_missing_ref_df.dtypes)

print("\nSite info columns and data types:")
print(site_info_df.dtypes)
```


---

# **4. Dataset Shape and Data Types**


---

## Code Cell 5

```python
print("Source PM2.5 dataset")
print("Rows:", source_df.shape[0])
print("Columns:", source_df.shape[1])
print(source_df.dtypes)

print("\nCapstone missing-reference dataset")
print("Rows:", capstone_missing_ref_df.shape[0])
print("Columns:", capstone_missing_ref_df.shape[1])
print(capstone_missing_ref_df.dtypes)

print("\nSite info dataset")
print("Rows:", site_info_df.shape[0])
print("Columns:", site_info_df.shape[1])
print(site_info_df.dtypes)
```


---

# **5. Timestamp Verification and Cleaning**


---

## Code Cell 6

```python
# Parse timestamps from the original clean source dataset.
source_df["ObservationTimeUTC"] = pd.to_datetime(
    source_df["ObservationTimeUTC"],
    errors="coerce"
)

print("Missing parsed timestamps:", source_df["ObservationTimeUTC"].isna().sum())
print("Minimum timestamp:", source_df["ObservationTimeUTC"].min())
print("Maximum timestamp:", source_df["ObservationTimeUTC"].max())

print("\nSample timestamps:")
display(source_df[["ID", "SiteID", "ObservationTimeUTC", "Value"]].head(10))
```


---

## Code Cell 7

```python

# to verify hours are preserved
print(source_df["ObservationTimeUTC"].dt.hour.value_counts().sort_index())
print("Unique timestamps:", source_df["ObservationTimeUTC"].nunique())
print("Expected hourly range sample:")
display(source_df[["ID", "ObservationTimeUTC"]].head(30))
```


---

# **6. Create Active Capstone Working Dataset**


---

This step uses the original clean dataset as the timestamp-safe source, then applies the missing-value pattern from the capstone reference file.


---

## Code Cell 8

```python
# Copy the clean timestamp-safe source dataframe.
working_df = source_df.copy()

# Make sure ID fields align.
working_df["ID"] = working_df["ID"].astype(str)
capstone_missing_ref_df["ID"] = capstone_missing_ref_df["ID"].astype(str)

# Identify which IDs have missing Value in the capstone reference dataset.
missing_value_ids = capstone_missing_ref_df.loc[
    capstone_missing_ref_df["Value"].isna(), "ID"
]

print("Number of controlled missing Value records:", len(missing_value_ids))

# Apply the same missing pattern to the working dataframe.
working_df.loc[working_df["ID"].isin(missing_value_ids), "Value"] = np.nan

print("Active working dataset created.")
print("Rows:", working_df.shape[0])
print("Columns:", working_df.shape[1])
print("Missing Value count:", working_df["Value"].isna().sum())
print("Missing Value percentage:", round(working_df["Value"].isna().mean() * 100, 2), "%")
```


---

## **6B. Create Date, Hour, and Day columns**


---

## Code Cell 9

```python
# Create time fields for later EDA

working_df["ObservationTimeUTC"] = pd.to_datetime(
    working_df["ObservationTimeUTC"],
    errors="coerce"
)

working_df["Date"] = working_df["ObservationTimeUTC"].dt.date
working_df["Hour"] = working_df["ObservationTimeUTC"].dt.hour
working_df["Day"] = working_df["ObservationTimeUTC"].dt.day

display(working_df[["ObservationTimeUTC", "Date", "Hour", "Day"]].head())
```


---

# **7. Clean Site Metadata and Merge**


---

## Code Cell 10

```python
print("Site info missing values before cleaning:")
print(site_info_df.isna().sum())

# Drop metadata rows without SiteID.
# This removes the DEC Monitor Average row because it does not map to a street-level SiteID.
site_info_clean = site_info_df.dropna(subset=["SiteID"]).copy()

# Align SiteID types before merge.
working_df["SiteID"] = working_df["SiteID"].astype(str).str.strip()
site_info_clean["SiteID"] = site_info_clean["SiteID"].astype(str).str.strip()

# Merge station metadata.
working_df = working_df.merge(
    site_info_clean,
    on="SiteID",
    how="left"
)

print("\nWorking dataframe after merge:", working_df.shape)
print("Columns after merge:")
print(working_df.columns.tolist())

print("\nMissing Location values after merge:", working_df["Location"].isna().sum())

display(working_df.head())
```


---

# **8. Summary Statistics Before Imputation**


---

## Code Cell 11

```python
print("Summary statistics before imputation:")
display(working_df["Value"].describe())
```


---

## Code Cell 12

```python
print("Summary by location before imputation:")
display(
    working_df.groupby("Location")["Value"]
    .agg(["count", "mean", "median", "min", "max", "std"])
    .sort_values("mean", ascending=False)
)
```


---

**An interpretive note:** Midtown West has only 39 non-missing records prior to imputation; therefore, it will be retained in the analysis but clearly labeled as a comparison station with a low record count, rather than treating its average as equally robust as that of the complete stations.


---

# **9. Missing-Value Analysis**


---

## Code Cell 13

```python
print("Missing values by column:")
display(working_df.isna().sum())
```


---

# **10. Imputation**


---

I use median imputation by station/location because PM2.5 values may vary by monitor location and the median is less sensitive to spikes than the mean.


---

## Code Cell 14

```python
# Preserve original Value column before imputation.
working_df["Value_Original_With_Missing"] = working_df["Value"]

# Station-level median imputation.
working_df["Value_Imputed"] = working_df.groupby("Location")["Value"].transform(
    lambda x: x.fillna(x.median())
)

# Fallback in case any station has all missing values.
overall_median = working_df["Value"].median()
working_df["Value_Imputed"] = working_df["Value_Imputed"].fillna(overall_median)

print("Missing Value before imputation:", working_df["Value"].isna().sum())
print("Missing Value_Imputed after imputation:", working_df["Value_Imputed"].isna().sum())
```


---

# **11. Verify Clean Data**


---

## Code Cell 15

```python
print("Missing values after imputation:")
display(working_df.isna().sum())
```


---

Value and Value_Original_With_Missing still show 109 missing values by design, while Value_Imputed is the clean analysis column used for the rest of the EDA.


---

## Code Cell 16

```python
print("Imputed records preview:")
display(
    working_df[working_df["Value_Original_With_Missing"].isna()]
    [["ID", "SiteID", "Location", "ObservationTimeUTC", "Value_Original_With_Missing", "Value_Imputed"]]
    .head(20)
)
```


---

## Code Cell 17

```python
print("Clean-data summary statistics:")
display(working_df["Value_Imputed"].describe())
```


---

# **12. Univariate Analysis 1: PM2.5 Distribution**


---

## Code Cell 18

```python
plt.figure(figsize=(10, 6))
plt.hist(working_df["Value_Imputed"], bins=30)
plt.title("Distribution of PM2.5 Values After Imputation")
plt.xlabel("PM2.5 (µg/m³)")
plt.ylabel("Frequency")
plt.show()
```


---

# **13. Univariate Analysis 2: PM2.5 by Hour of Day**


---

## Code Cell 19

```python
hourly_avg = working_df.groupby("Hour")["Value_Imputed"].mean()

plt.figure(figsize=(10, 6))
plt.plot(hourly_avg.index, hourly_avg.values, marker="o")
plt.title("Average PM2.5 by Hour of Day")
plt.xlabel("Hour of Day")
plt.ylabel("Average PM2.5 (µg/m³)")
plt.xticks(range(0, 24))
plt.grid(True)
plt.show()

display(hourly_avg.reset_index(name="Average_PM25"))
```


---

## **13B. Univariate Boxplot: Overall PM2.5 Distribution**


---

## Code Cell 20

```python
# 13B. Univariate Boxplot: Overall PM2.5 Distribution

plt.figure(figsize=(8, 5))
plt.boxplot(working_df["Value_Imputed"].dropna(), vert=False)
plt.title("Boxplot of PM2.5 Values After Imputation")
plt.xlabel("PM2.5 (µg/m³)")
plt.show()
```


---

# **14. Bivariate Analysis 1: PM2.5 by Location**


---

## Code Cell 21

```python
location_avg = (
    working_df.groupby("Location")["Value_Imputed"]
    .mean()
    .sort_values(ascending=False)
)

plt.figure(figsize=(12, 7))
plt.bar(location_avg.index, location_avg.values)
plt.title("Average PM2.5 by Monitoring Location")
plt.xlabel("Location")
plt.ylabel("Average PM2.5 (µg/m³)")
plt.xticks(rotation=90)
plt.tight_layout()
plt.show()

display(location_avg.reset_index(name="Average_PM25"))
```


---

# **15. Bivariate Analysis 2: PM2.5 Over Time**


---

## Code Cell 22

```python
daily_avg = (
    working_df.groupby("Date")["Value_Imputed"]
    .mean()
    .reset_index(name="Average_PM25")
)

plt.figure(figsize=(12, 6))
plt.plot(daily_avg["Date"], daily_avg["Average_PM25"], marker="o")
plt.title("Daily Average PM2.5 Over Time")
plt.xlabel("Date")
plt.ylabel("Average PM2.5 (µg/m³)")
plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()

display(daily_avg)
```


---

## **15B. Bivariate Boxplot: PM2.5 by Location**


---

## Code Cell 23

```python
# 15B. Bivariate Boxplot: PM2.5 Distribution by Location

# Prepare data grouped by location
locations_sorted = (
    working_df.groupby("Location")["Value_Imputed"]
    .median()
    .sort_values(ascending=False)
    .index
)

boxplot_data = [
    working_df.loc[working_df["Location"] == location, "Value_Imputed"].dropna()
    for location in locations_sorted
]

plt.figure(figsize=(12, 7))
plt.boxplot(boxplot_data, labels=locations_sorted, vert=False)
plt.title("Boxplot of PM2.5 Values by Monitoring Location")
plt.xlabel("PM2.5 (µg/m³)")
plt.ylabel("Monitoring Location")
plt.tight_layout()
plt.show()
```


---

# **16. Threshold Analysis: 24-Hour Standard**


---

The 24-hour standard used here is 35 µg/m³.


---

## Code Cell 24

```python
STANDARD_24H = 35

# Daily average by location.
daily_location_avg = (
    working_df.groupby(["Date", "Location"])["Value_Imputed"]
    .mean()
    .reset_index(name="Daily_Location_Avg_PM25")
)

daily_location_avg["Above_35"] = daily_location_avg["Daily_Location_Avg_PM25"] > STANDARD_24H

print("Number of daily location averages above 35 µg/m³:")
print(daily_location_avg["Above_35"].sum())

display(
    daily_location_avg.sort_values("Daily_Location_Avg_PM25", ascending=False).head(10)
)
```


---

# **17. Short-Term Spike Flag**


---

This is a lightweight clinical attention marker, not a legal compliance threshold.


---

## Code Cell 25

```python
SPIKE_ATTENTION_LEVEL = 10

working_df["At_Or_Above_10"] = working_df["Value_Imputed"] >= SPIKE_ATTENTION_LEVEL

print("Hourly records at or above 10 µg/m³:")
print(working_df["At_Or_Above_10"].sum())

display(
    working_df[working_df["At_Or_Above_10"]]
    [["ID", "Location", "ObservationTimeUTC", "Value_Imputed"]]
    .sort_values("Value_Imputed", ascending=False)
    .head(20)
)
```


---

## Code Cell 26

```python
# Hour-to-hour increase by station.
working_df = working_df.sort_values(["Location", "ObservationTimeUTC"])

working_df["Previous_PM25"] = working_df.groupby("Location")["Value_Imputed"].shift(1)
working_df["Hour_To_Hour_Change"] = working_df["Value_Imputed"] - working_df["Previous_PM25"]
working_df["Increase_10_Or_More"] = working_df["Hour_To_Hour_Change"] >= 10

print("Number of hour-to-hour increases of 10 µg/m³ or more:")
print(working_df["Increase_10_Or_More"].sum())

display(
    working_df[working_df["Increase_10_Or_More"]]
    [["Location", "ObservationTimeUTC", "Previous_PM25", "Value_Imputed", "Hour_To_Hour_Change"]]
    .sort_values("Hour_To_Hour_Change", ascending=False)
    .head(20)
)
```


---

# **18. Highest Peaks and Lowest Levels**


---

## Code Cell 27

```python
print("Top 10 highest PM2.5 readings:")
display(
    working_df[["ID", "Location", "ObservationTimeUTC", "Value_Imputed"]]
    .sort_values("Value_Imputed", ascending=False)
    .head(10)
)

#
print("Top 10 lowest PM2.5 readings:")
display(
    working_df[["ID", "Location", "ObservationTimeUTC", "Value_Imputed"]]
    .sort_values("Value_Imputed", ascending=True)
    .head(10)
)
```


---

# **19. Midtown West Low-Record Check**


---

## Code Cell 28

```python
location_counts = working_df.groupby("Location").size().sort_values()

print("Record count by location:")
display(location_counts.reset_index(name="Record_Count"))

print("Low-record locations:")
display(location_counts[location_counts < 100].reset_index(name="Record_Count"))
```


---

# **20. Correlation Matrix**


---

## Code Cell 29

```python
# Select numeric fields relevant to the analysis.
corr_df = working_df[[
    "Value_Imputed",
    "Hour",
    "Day",
    "Latitude",
    "Longitude"
]].copy()

correlation_matrix = corr_df.corr(numeric_only=True)

print("Correlation matrix:")
display(correlation_matrix)
```


---

## Code Cell 30

```python
plt.figure(figsize=(8, 6))
plt.imshow(correlation_matrix)
plt.colorbar()
plt.xticks(range(len(correlation_matrix.columns)), correlation_matrix.columns, rotation=45, ha="right")
plt.yticks(range(len(correlation_matrix.columns)), correlation_matrix.columns)
plt.title("Correlation Matrix")
plt.tight_layout()
plt.show()
```


---

# **Findings, Insights, Recommendations.**

## **Findings**

This analysis examined hourly PM2.5 measurements from New York City monitoring stations from May 1 to May 19, 2026. The active capstone dataset included a controlled missing-value case so that missing-value detection, imputation, and clean-data verification could be demonstrated.

- After imputation, the dataset contained **5,608 records** with **0 missing values** in the analysis column, `Value_Imputed`. Before imputation, there were **109 intentionally missing PM2.5 values**, representing approximately **1.94%** of the dataset.

- Overall PM2.5 levels were moderate during the study period. The average PM2.5 value was approximately **6.69 µg/m³**, and the median was approximately **5.53 µg/m³**. The minimum observed PM2.5 value was **2.19 µg/m³**, and the maximum observed value was **32.10 µg/m³**.

- The highest PM2.5 reading occurred at **Manhattan Bridge** on **May 10, 2026 at 16:00**, with a value of **32.10 µg/m³**. The lowest PM2.5 reading was **2.19 µg/m³**, recorded at **Broadway/35th St** on **May 7, 2026 at 06:00**.

- No hourly readings exceeded **35 µg/m³**, and no daily location-level averages exceeded the **35 µg/m³ 24-hour standard**. However, there were **841 hourly records at or above 10 µg/m³**, and **4 hour-to-hour increases of 10 µg/m³ or more**, which suggests that short-term elevated PM2.5 periods occurred even though the 24-hour standard was not exceeded.

- Location-level results showed that **Midtown West** had the highest average PM2.5 value, followed by locations such as **Hamilton Bridge**, **Broadway/35th St**, **Cross Bronx Expy**, and **Hunt's Point**. However, Midtown West had only **40 records**, so it should be interpreted as a low-record comparison station rather than as the strongest location-level conclusion.

- Time-based analysis showed that **May 10, 2026** had the highest daily average PM2.5 level during the analysis period. The hourly analysis showed modest variation by hour of day, with the highest average hour around **15:00** and the lowest average hour around **6:00**.

- The boxplots showed that the PM2.5 distribution was right-skewed. Most values were concentrated in the lower range, while several higher readings appeared as upper outliers. These outliers support the finding that short-term elevated readings occurred during the study period.

## **Insights**

- The analysis suggests that NYC PM2.5 levels were generally below the 24-hour threshold during this period, but short-term increases still occurred. This is important because daily averages can hide temporary spikes that may matter for people who are sensitive to air pollution.

- The highest readings were concentrated around May 10, especially near bridge and expressway-related monitoring locations. This suggests that location and short-term conditions may influence PM2.5 levels, even when overall air quality remains below the 24-hour standard.

- The controlled imputation process worked well for this capstone project. Because only a small percentage of values were missing and the missing values were limited to the PM2.5 measurement column, median imputation by location preserved the overall structure of the dataset while satisfying the requirement to demonstrate imputation.

- The correlation matrix showed weak relationships between PM2.5 and the available numeric variables. This suggests that PM2.5 variation in this dataset may be influenced by factors not included in the current file, such as weather, traffic volume, wind direction, humidity, or local events.

## **Recommendations**

- Future versions of this project (AI Foundations II) should add more variables, such as weather data, traffic indicators, borough grouping, wind conditions, or longer date ranges. These additions would make it easier to explain why PM2.5 values increase on certain days or at certain locations.

- Midtown West should remain in the dataset, but its results should be interpreted cautiously because it has far fewer records than the other monitoring stations. It is useful as a comparison point, but not enough to support strong conclusions by itself.

- For public communication, the findings should emphasize that the dataset did not show 24-hour threshold exceedances, but short-term elevated readings were still present. This distinction keeps the interpretation accurate, practical, and health-aware.

## **Limitations**

- This analysis is exploratory and educational. It should not be treated as an official regulatory compliance assessment.

- The PM2.5 data are preliminary and subject to change. External factors can affect monitor functioning and may cause temporary data gaps.

- The capstone dataset intentionally included missing PM2.5 values for imputation practice. The missing-value process was controlled and limited, but the imputed values are still estimates rather than original measurements.

- The short-term spike flag at **10 µg/m³** was used as a lightweight attention marker, not as a legal standard. The primary threshold review remains the **35 µg/m³ 24-hour standard**.

- The correlation matrix was limited because the dataset contains only a small number of numeric explanatory variables. Additional variables would be needed for stronger causal or predictive analysis.
