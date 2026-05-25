# Limitations

## Purpose

This document explains the main limitations of the PM2.5 NYC Air Quality Capstone exploratory data analysis.

The project is intended as an academic and portfolio-based data analysis exercise. It should not be interpreted as a regulatory assessment, causal public health study, or complete air-quality evaluation.

## Educational Scope

This project was completed as part of a Capstone exercise.

The analysis was designed to demonstrate data analysis skills, including:

- data loading
- data inspection
- missing-value review
- imputation
- metadata merging
- exploratory analysis
- threshold analysis
- short-term spike flagging
- public health interpretation
- technical documentation

Because this was an academic exercise, some analysis choices were made to demonstrate skills rather than to produce an official environmental assessment.

## Intentional Missing-Data Exercise

A key limitation is that some data was intentionally removed or treated as missing for the Capstone Project.

The purpose of this step was to demonstrate the ability to:

- detect missing values
- evaluate missingness
- select an imputation strategy
- apply imputation transparently
- explain how missing-data decisions affect interpretation

This missing-data exercise should not be interpreted as proof that the original public health dataset was unusable, incomplete, or unreliable.

The imputation process was included to demonstrate technical skill in a controlled academic setting.

## Imputation Limitations

The project used median imputation by monitoring location.

This approach is more context-aware than applying one global replacement value, but it still has limitations.

Median imputation can:

- reduce natural variability
- smooth out extreme values
- affect summary statistics
- influence downstream comparisons
- make imputed values appear more certain than they really are

For that reason, imputed results should be interpreted cautiously.

## Time Period Limitation

The dataset represents a limited analysis period.

Patterns observed in this project may not represent:

- long-term PM2.5 trends
- seasonal changes
- annual exposure levels
- unusual pollution events outside the study period
- differences across years

A longer dataset would be needed to support stronger conclusions about long-term air quality patterns.

## Geographic and Monitoring Coverage Limitation

The analysis depends on the available monitoring sites and site metadata.

Monitoring stations do not necessarily capture conditions for every neighborhood, block, indoor environment, or individual exposure situation.

PM2.5 levels can vary based on:

- traffic
- building density
- industrial activity
- weather
- local emissions sources
- distance from monitoring equipment
- time of day

Therefore, results should not be generalized beyond the available monitoring context without additional evidence.

## Exploratory Analysis Limitation

This project is exploratory.

Exploratory data analysis is useful for identifying patterns, questions, and areas for further investigation, but it does not prove causation.

The analysis can suggest possible patterns in PM2.5 values, but it cannot determine the exact cause of those patterns without more data.

## Threshold Analysis Limitation

Threshold-based analysis helps make PM2.5 patterns easier to interpret.

However, thresholds are simplifications.

They can help identify observations of interest, but they do not capture every health, environmental, or regulatory nuance.

Any threshold interpretation should be checked against the final data source, citation requirements, and relevant air-quality guidance before publication.

## Short-Term Spike Flag Limitation

The short-term spike flag analysis identifies elevated observations for further review.

These flags should not be interpreted as confirmed pollution events.

A spike flag may be influenced by:

- local emissions
- weather conditions
- instrument behavior
- data processing choices
- missing-value handling
- short-term variation

Additional context would be needed to explain why a specific spike occurred.

## Public Health Interpretation Limitation

PM2.5 is associated with respiratory and cardiovascular health concerns.

However, this project does not estimate individual exposure, diagnose health outcomes, or measure direct health effects.

The public health interpretation is included to explain why PM2.5 matters and to practice responsible communication.

The findings should be interpreted as exploratory and educational, not as a formal health-risk assessment.

## Data Verification Limitation

Before public release, the final notebook should be checked for:

```text
final data paths
final row counts
final missing-value counts
final imputation results
final threshold counts
final spike-flag counts
final charts
final source citations
```

Any public claims should match the final notebook outputs exactly.

## Reproducibility Limitation

The analysis is reproducible only if the same data files, notebook version, and processing steps are used.

Changes to the dataset, file paths, notebook code, or imputation strategy may produce different results.

## Summary

This project is best understood as a portfolio-ready exploratory data analysis and academic skills demonstration.

Its strongest value is showing a clear technical workflow:

```text
load data → inspect data → handle missing values → merge metadata → analyze patterns → communicate findings responsibly
```

The results should be used for learning, portfolio review, and discussion rather than official regulatory or causal public health conclusions.
