# Findings Summary

## Purpose

This document summarizes the main findings from the PM2.5 NYC Air Quality Capstone exploratory data analysis.

The findings are written for a public-facing portfolio audience and should be reviewed against the final notebook before publication.

## Project Focus

This project explored PM2.5 air quality data in New York City using Python and Google Colab.

The analysis focused on:

- PM2.5 concentration patterns
- monitoring-site differences
- missing-value handling
- threshold-based interpretation
- short-term concentration spikes
- public health communication

## Main Finding 1 — PM2.5 Values Vary Across Observations

The analysis showed variation in PM2.5 measurements across the dataset.

This variation is important because PM2.5 levels can change based on location, time, weather, emissions activity, and monitoring conditions.

The project used exploratory analysis to review these differences before making interpretation claims.

## Main Finding 2 — Monitoring Location Matters

The project used monitoring site metadata to support location-based interpretation.

By connecting PM2.5 measurements with monitoring-site information, the analysis could compare patterns across sites more clearly.

This helped avoid treating all observations as if they came from the same location or monitoring context.

## Main Finding 3 — Missing Values Required a Controlled Strategy

The dataset included missing values that needed to be reviewed before analysis.

Instead of ignoring missingness or applying a single global replacement value, the project used a controlled strategy based on monitoring location.

Median imputation by monitoring location helped preserve local patterns more responsibly than one-size-fits-all imputation.

## Main Finding 4 — Threshold Analysis Helped Make PM2.5 Patterns Easier to Interpret

Threshold-based analysis made it easier to identify observations that may be more meaningful from an air-quality or public health perspective.

Rather than only describing raw PM2.5 values, the analysis used thresholds to support clearer interpretation.

This helped translate technical results into a format that is easier to communicate to a general audience.

## Main Finding 5 — Short-Term Spike Flags Identified Elevated Observations

The notebook included a short-term spike flag analysis.

This helped identify observations where PM2.5 levels appeared elevated compared with surrounding or expected patterns.

These flags are useful for exploratory review, but they should not be interpreted as proof of a specific pollution event without additional evidence.

## Main Finding 6 — The Analysis Demonstrates a Complete EDA Workflow

Beyond the environmental findings, the project demonstrates a complete data analysis workflow.

The notebook shows the ability to:

- load and inspect raw data
- evaluate missing values
- clean and prepare data
- merge metadata
- analyze distributions
- compare variables
- create interpretable flags
- summarize results
- document limitations

This makes the project useful as a technical portfolio artifact.

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
- visual analysis
- threshold-based interpretation
- communication of findings
- documentation for a public technical audience

## Publication Note

Before this file is published publicly, add or verify:

```text
specific PM2.5 summary statistics
specific chart references
specific site-level comparisons
final threshold counts
final spike-flag counts
source citation details
```

## Summary

The PM2.5 Capstone analysis shows how raw environmental data can be cleaned, structured, analyzed, interpreted, and communicated.

The strongest value of the project is that it connects technical Python analysis with clear environmental and public health storytelling.
