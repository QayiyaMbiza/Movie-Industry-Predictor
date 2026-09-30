# Prediction results and limitations

## Results
- Consolidated predictions: 19.
- Missing production budgets: 9.
- Movies with budget sensitivity analysis: 9.
- Classification changes across the budget grid: 8 of 9.
- Classification probability threshold: 0.57.

## Interpretation
The baseline uses the saved training preprocessing. Missing budgets remain
missing in the source features and are imputed by the saved preprocessor.
The effective $20 million budget fallback is not an estimate of a film's
actual production cost.

Budget research did not establish sufficiently reliable production budgets
for the nine movies reviewed. Unverified claims were not substituted into
the baseline.

Sensitivity results show how model outputs vary under hypothetical budget
inputs while other inputs remain fixed. Their minimum and maximum values
are not confidence intervals, forecast bounds, or causal estimates.
The proportion of scenarios classified as blockbusters is not a probability.

## Limitations
- Historical modelling data ends in 2020; later releases may differ.
- Director, star, and company histories reflect dataset coverage.
  No recorded history does not necessarily mean no real-world experience.
- Franchise, marketing, and audience sentiment are not model inputs.
- Missing-budget forecasts remain provisional.
- Released-film predictions must be distinguished from future forecasts.
  Retrospective predictions are not evidence of a prospective backtest
  unless input availability before release is established.
- Revenue predictions represent worldwide box office, not studio profit.
- Release dates and other metadata require review before publication.

## Updating predictions
When credible budgets become available, update the source data, rerun
feature engineering, apply the saved preprocessor, and regenerate predictions.
New prediction inputs alone do not require retraining.
