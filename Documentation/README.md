# Movie Industry Predictor

A machine-learning project that estimates worldwide movie box-office revenue and classifies whether a film is likely to be a blockbuster using information available before release.

## 1. Project overview and business problem

Film producers, distributors, studios and marketing teams make financial decisions before a movie is released, often under significant uncertainty.

This project investigates whether pre-release information can support:

- Worldwide box-office revenue estimates.
- Blockbuster-risk classification.
- Budget and release-planning analysis.
- Structured comparison of upcoming films.
- Scenario analysis when production budgets are uncertain.

The model is a decision-support tool. It does not guarantee commercial success, profit or return on investment.

## 2. Prediction targets

### Revenue regression

The regression model predicts worldwide box-office revenue.

Because revenue is highly right-skewed, the modelling target is:

```text
log1p(worldwide_box_office)