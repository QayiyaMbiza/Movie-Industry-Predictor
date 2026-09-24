# Source Data Audit

## Purpose

This notebook evaluates the quality, completeness, coverage, and compatibility of the source datasets being considered for the **Predicting Movie Box Office Success Using Machine Learning** project.

The audit is performed before any major cleaning, transformation, deduplication, or dataset integration.

## Datasets Audited

### Dataset A — Cleaned Industry Dataset

* 7,668 movies
* 20 columns
* Release-year coverage: 1980–2020
* 5,497 movies with positive budget values
* 7,479 movies with positive revenue values
* 5,436 movies with both budget and revenue

### Dataset B — TMDB Relational Dataset

* 9,771 movies
* 22 columns
* Release-year coverage: 1904–2028
* 4,015 movies with positive budget values
* 4,027 movies with positive revenue values
* 3,435 movies with both budget and revenue

### Dataset C — TMDB Recent Dataset

* 17,978 movies
* 16 columns
* Release-year coverage: 2010–2025
* 5,515 movies with positive budget values
* 6,822 movies with positive revenue values
* 4,105 movies with both budget and revenue

## Audits Performed

The notebook currently includes:

* Basic dataset structure audits
* Missing-value inspection
* Duplicate-row inspection
* Budget data-quality audit
* Revenue data-quality audit
* Complete financial-record counts
* Release-year coverage
* Duplicate-title analysis
* Movie ID validation
* Dataset summary table
* Summary validation
* Cross-dataset matching-key creation
* Matching-key quality analysis
* Cross-dataset movie-overlap analysis
* Duplicate matching-key investigation

## Cross-Dataset Matching

Movies are provisionally matched using a normalized combination of:

`movie title + release year`

### Current overlap

* Dataset A ∩ Dataset B: 2,367 movies
* Dataset A ∩ Dataset C: 1,808 movies
* Dataset B ∩ Dataset C: 3,052 movies
* Dataset A ∩ Dataset B ∩ Dataset C: 957 movies

## Preliminary Findings

Dataset A currently provides the strongest historical financial coverage and is a strong candidate for the core historical modelling dataset.

Datasets B and C provide substantially broader movie coverage and may be used to enrich the modelling dataset with additional movie information.

TMDB financial fields require special handling because unknown budget and revenue values are frequently represented as zero.

The datasets must not simply be concatenated because thousands of movies occur in more than one source.

Cross-dataset matching and source-priority rules will therefore be established before dataset integration.

## Current Status

**Source Audit: In Progress**

The next task is to investigate duplicate matching keys and unmatched records before finalizing the cross-dataset integration strategy.


