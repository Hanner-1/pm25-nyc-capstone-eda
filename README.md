# PM2.5 NYC Air Quality Capstone EDA

## Project Overview

This project is an exploratory data analysis of PM2.5 air quality data in New York City.

The goal is to demonstrate a complete data analysis workflow using Python, Google Colab, pandas, NumPy, visualization, data cleaning, missing-value handling, threshold analysis, and public health interpretation.

This repository is the public technical component of a larger workforce development portfolio.

## Repository Structure

```text
pm25-nyc-capstone-eda/
├── README.md
├── notebook/
│   └── User_PythonAnalysis_Capstone.ipynb
├── data/
│   ├── PM2.5_may_2026.csv
│   ├── PM2.5_may_2026_Capstone.csv
│   └── site_info.csv
└── docs/
    ├── methodology.md
    ├── findings_summary.md
    ├── limitations.md
    └── citation.md
```

## Project Files

|File|Purpose|
|-|-|
|`notebook/User_PythonAnalysis_Capstone.ipynb`|Main Google Colab notebook and technical analysis|
|`data/PM2.5_may_2026.csv`|PM2.5 data source file|
|`data/PM2.5_may_2026_Capstone.csv`|Capstone working data file|
|`data/site_info.csv`|Monitoring site metadata|
|`docs/methodology.md`|Summary of the analysis process|
|`docs/findings_summary.md`|Summary of major findings|
|`docs/limitations.md`|Known limitations and interpretation cautions|
|`docs/citation.md`|Data and source citation information|

## Tools and Skills Demonstrated

This project demonstrates:

* Python data analysis
* Google Colab notebook development
* pandas and NumPy
* Data loading and inspection
* Timestamp verification
* Missing-value review
* Median imputation by monitoring location
* Metadata merging
* Univariate and bivariate exploratory analysis
* Threshold and short-term spike analysis
* Data visualization
* Public health interpretation
* Technical documentation
* Portfolio-ready project communication

## Analysis Workflow

The notebook follows a structured exploratory data analysis workflow:

1. Load PM2.5 and monitoring site data.
2. Inspect the raw datasets.
3. Verify timestamps and data structure.
4. Review missing values.
5. Apply a controlled missing-value strategy.
6. Merge monitoring metadata.
7. Conduct univariate analysis.
8. Conduct bivariate analysis.
9. Analyze PM2.5 threshold patterns.
10. Flag short-term concentration spikes.
11. Summarize findings, recommendations, limitations, and citations.

## Public Health Context

PM2.5 refers to fine particulate matter that can affect respiratory and cardiovascular health. This project uses PM2.5 data to practice environmental data analysis and communicate air-quality patterns in a clear, portfolio-ready format.

## Intended Audience

This project is designed for:

* Data analyst portfolio reviewers
* Workforce development evaluators
* Hiring managers
* Interview preparation
* Public health and environmental data storytelling practice

