# Prediction Target Data Collection Report

## Project

**Predicting Movie Box Office Success Using Machine Learning**

## Stage

Prediction Target Data Collection

## Purpose

This stage created a structured dataset for the movies selected for
box-office prediction and backtesting.

The collection process focused primarily on information that would
reasonably be available before theatrical release.

## Prediction Pool

A total of **19 movies** were retained.

- Released / backtest movies: **10**
- Future prediction movies: **9**

The released cohort will later be used to compare model predictions
against observed worldwide theatrical performance.

The future cohort represents the films for which final box-office
predictions will be generated.

## Data Collected

The collection stage gathered or derived:

- Release dates and release years
- Studios and distributors
- Production companies
- Genres
- Directors
- Lead cast
- Production budgets where credibly available
- Franchise and sequel status
- Existing-IP classification
- Franchise gap years
- Release timing
- Director historical track record
- Cast historical track record
- Pre-release sentiment
- Pre-release audience-interest proxy
- Worldwide box-office outcomes for released backtest films

## Data Availability

Production budget available:

**10 / 19 movies**

Pre-release sentiment available:

**16 / 19 movies**

Audience-interest proxy available:

**19 / 19 movies**

Worldwide box-office outcome available:

**10 / 19 movies**

## Source Strategy

Official studio, distributor and production-company sources were
preferred wherever possible.

Major sources included:

- Sony Pictures
- Sony Pictures Animation
- Disney
- Marvel Studios
- Pixar
- 20th Century Studios
- Universal Pictures
- DreamWorks Animation
- Warner Bros.
- DC Studios
- Paramount Pictures
- Legendary Entertainment
- Nintendo
- A24
- Lionsgate

Reputable industry and box-office sources were used where official
sources did not provide sufficient information, including:

- Variety
- The Numbers
- Rotten Tomatoes

A machine-readable source register was saved alongside the completed
prediction-target dataset.

## Data Quality Decisions

Unknown values were retained as missing rather than estimated without
credible evidence.

Reported and estimated budgets were labelled separately.

Future movies contain no worldwide box-office outcome values.

Pre-release sentiment and audience-interest scores were collected
without using post-release audience or box-office results.

## Modelling Caution

Pre-release sentiment and audience-interest variables are currently
candidate features.

They should only be included in the final machine-learning model if
comparable historical versions can be created for the model-training
dataset. Otherwise they may be retained for descriptive analysis and
prediction interpretation.

## Excluded / Later-Bin Targets

The following previously considered films were removed from the active
prediction pool:

- Narnia
- Untitled Paranormal Activity film
- Star Wars: The Mandalorian and Grogu

These were excluded because the available information or theatrical
comparability was considered insufficient for the current modelling
objective.

## Outputs

The stage produced:

1. `prediction_targets_complete.csv`
2. `prediction_target_source_register.csv`
3. `03_prediction_target_data_collection_report.md`

## Stage Status

**Prediction Target Data Collection: COMPLETE**

## NextUp

Proceed to **Notebook 04 — Feature Engineering**.

The next stage will transform the collected historical and target data
into consistent numerical and categorical features suitable for
machine-learning models.