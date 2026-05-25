# Methodology

## Purpose

This document summarizes the methodology used in the PM2.5 NYC Air Quality Capstone exploratory data analysis.

The goal of the project was to practice and demonstrate a clear data analysis workflow using Python and Google Colab, while interpreting PM2.5 air quality data in a public health context.

## Tools Used

The analysis was completed using:

- Google Colab
- Python
- pandas
- NumPy
- data visualization tools
- Markdown documentation

## Data Sources

The project uses PM2.5 air quality data and monitoring site metadata.

Primary project files include:

```text
PM2.5_may_2026.csv
PM2.5_may_2026_Capstone.csv
site_info.csv
```

The PM2.5 files contain air quality measurements. The site information file provides monitoring site metadata used to support location-based interpretation.

## General Workflow

The notebook follows a structured exploratory data analysis workflow:

```text
1. Load data
2. Inspect raw data
3. Review structure and timestamps
4. Check missing values
5. Apply a controlled missing-value strategy
6. Merge monitoring site metadata
7. Conduct univariate analysis
8. Conduct bivariate analysis
9. Analyze PM2.5 threshold patterns
10. Flag short-term concentration spikes
11. Summarize findings and limitations
```

## Step 1 — Data Loading

The analysis begins by loading the PM2.5 datasets and the monitoring site metadata into the notebook.

This step confirms that the files can be accessed, read, and prepared for analysis in Python.

## Step 2 — Initial Data Inspection

The raw datasets are inspected to understand:

- number of rows and columns
- column names
- data types
- basic descriptive statistics
- missing values
- timestamp fields
- site or location identifiers

This inspection helps identify what cleaning and preparation steps are needed before analysis.

## Step 3 — Timestamp Review

Timestamp fields are reviewed to confirm that the data represents the expected time period and that the observations can be interpreted in sequence.

This step supports later analysis of short-term changes and potential PM2.5 concentration spikes.

## Step 4 — Missing-Value Review

Missing values are reviewed before cleaning.

The purpose is to understand:

- which columns contain missing values
- how much data is missing
- whether missingness affects key PM2.5 measurements
- whether missing values are related to monitoring locations

## Step 5 — Missing-Value Strategy

A controlled missing-value strategy is applied.

Where appropriate, missing PM2.5 values are handled using median imputation by monitoring location.

This approach keeps the imputation tied to local monitoring patterns rather than applying one global replacement value across the entire dataset.

## Step 6 — Metadata Merge

Monitoring site metadata is merged into the PM2.5 dataset.

This supports analysis by location and helps make the dataset easier to interpret.

The merge allows the notebook to connect measurements with monitoring-site information.

## Step 7 — Univariate Analysis

Univariate analysis is used to examine individual variables.

This includes reviewing distributions, summary statistics, and patterns in PM2.5 measurements.

The purpose is to understand the basic shape and range of the data before comparing variables.

## Step 8 — Bivariate Analysis

Bivariate analysis is used to explore relationships between variables.

This may include comparisons across monitoring sites, time periods, or other available fields.

The purpose is to identify patterns that are not visible from single-variable summaries alone.

## Step 9 — Threshold Analysis

The notebook includes threshold-based analysis of PM2.5 values.

This step helps identify observations that may be more meaningful from an air-quality or public-health interpretation perspective.

Threshold analysis supports clearer communication because it connects raw measurements to interpretable categories or flags.

## Step 10 — Short-Term Spike Flagging

The notebook flags short-term PM2.5 concentration spikes.

This step helps identify observations or periods where PM2.5 values appear elevated compared with surrounding or expected patterns.

Spike flagging is useful for exploratory analysis, but it should not be interpreted as proof of a specific pollution event without additional context.

## Step 11 — Findings and Interpretation

The final stage summarizes:

- major findings
- observed PM2.5 patterns
- recommendations
- limitations
- citation notes

The interpretation is written for a portfolio audience and emphasizes both technical skill and responsible communication.

## Reproducibility Notes

To reproduce the analysis:

1. Open the notebook in Google Colab or a local Jupyter environment.
2. Confirm that the data files are available in the expected folder or URL paths.
3. Run the notebook cells in order.
4. Review outputs, charts, and summary tables.
5. Compare the results with the findings and limitations documents.

## Methodological Cautions

This project is exploratory.

The analysis should not be treated as a regulatory determination, causal study, or complete air-quality assessment.

Important cautions:

- The dataset may represent a limited time period.
- Missing-value handling can affect results.
- Monitoring-site coverage may not represent all NYC neighborhoods equally.
- Short-term spike flags require additional context before interpretation.
- PM2.5 patterns should be interpreted alongside weather, emissions, geography, and monitoring conditions when available.

## Summary

This methodology demonstrates a complete beginner-to-intermediate data analysis workflow:

```text
load → inspect → clean → merge → analyze → interpret → document
```

The project emphasizes technical clarity, responsible environmental interpretation, and portfolio-ready communication.
