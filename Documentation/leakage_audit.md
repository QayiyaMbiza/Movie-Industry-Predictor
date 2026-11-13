# Data Leakage Audit

## Scope

This audit evaluates whether any information unavailable before a film's release enters the model features, preprocessing, train/test split or evaluation.

## Findings that pass

- Worldwide box office and blockbuster labels are excluded from the predictor columns.
- The project uses a chronological split:
  - Training: through 2015
  - Validation: 2016–2018
  - Test: 2019–2020
- Numerical imputation and scaling are fitted on training data only.
- Categorical encoding is fitted on training data only.
- The prediction notebook loads saved models and does not retrain them.
- Sentiment is not included in the active production model.

## Issue 1: Historical track-record construction

Historical director, lead-star and production-company features are currently created using a condition equivalent to:

```sql
prior.release_year < current.release_year