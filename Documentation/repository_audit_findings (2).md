# Movie Industry Predictor repository audit

Audit date: 30 September 2026  
Repository: https://github.com/QayiyaMbiza/Movie-Industry-Predictor  
Commit inspected: `9a40428caf9fc7e92cec408d40134d1e3215ff59`  
Status: First audit pass complete; updated prediction notebook recovered and export corrections checked. Final report is not yet ready for sign-off.

## Scope and verification boundary

Read the four original documentation reports and inspected repository notebooks, CSVs, model metadata, SQLite tables, saved evaluation results and available chart files. Recomputed metrics from all 211 saved test predictions and checked all 81 sensitivity records against their summary and baseline. Inspected notebook source and stored errors. Did not execute the entire notebook chain, load saved estimators under a different scikit-learn version, or independently reverify current film facts and source articles. No repository files were changed or pushed.

## Priority findings

1. **The final report contains six recoverable forecast placeholders.** All nine baseline forecasts are available in `Data/Results/Prediction_Final/prediction_results_consolidated.csv`. Unknown production budgets and omitted revenue predictions are different issues.
2. **Two final model families exist.** Notebook 08 trains the final estimators on train plus validation, saving under `Models/Final` and `Data/Model_Results/Final`. Notebook 09 trains on train only, saving under `Data/Models/Final` and `Data/Results/Final_Evaluation`. The latter is identified by the prediction run metadata as the inference family. Their test results must not be combined. The test set has been scored for both configurations; wording claiming a single untouched evaluation across the entire project needs correction. This alone does not prove test-driven tuning.
3. **The committed prediction notebook is incomplete and contains a stored error.** `Notebooks/upcoming_movie_predictions.ipynb` has 13 physical notebook cells, ending at feature validation with a missing-column ValueError. Code for the successful scoring run, sensitivity, consolidation and final charts is absent from that notebook. Exported outputs exist, but the current committed notebook cannot reproduce them from start to finish.
4. **A complete collection notebook is misplaced relative to the apparent top-level sequence.** `Notebooks/03_prediction_target_data_collection.ipynb` stops after the initial target register and contains empty cells. `Notebooks/Data/Audit/03_prediction_target_data_collection.ipynb` includes budget collection, outcomes, sentiment, validation and completed exports. Do not overwrite the completed copy with the shorter one. Duplicate audit/integration/feature notebooks also need an explicit canonical execution order.
5. **Missing regression evidence is available.** Deployed-family test revenue MAE is $93,230,795.75, RMSE $216,801,644.55, log MAE 1.437716, log RMSE 2.005540 and log R² 0.542676. These values reproduce from the saved row-level results.
6. **The classifier target definition is recoverable.** A blockbuster is at or above the training-period 90th revenue percentile, $183,907,649.60. This dollar threshold is distinct from the 0.57 probability decision threshold. Train/validation/test contain 6,668 / 600 / 211 films from 1980–2015 / 2016–2018 / 2019–2020. Notebook 06 briefly calculates a full-data threshold, then replaces it with the training-only threshold before export.
7. **The final exports lose provenance.** The consolidated CSV omits `cohort`, actual budget and budget type. These can be restored through a validated title join with the target register. Budget availability alone does not establish input reliability: Toy Story 5 uses a provisional $175 million midpoint of a stated $150–200 million range; Michael uses a reported minimum. These labels must survive into reporting.
8. **Status text conflicts with verified indicator flags.** The latest run metadata sets `indicator_definitions_verified` to true but retains `provisional_indicator_definitions` and a limitation saying definitions await verification. Correct the generating code, not just a displayed label.
9. **The report overstates multi-source training integration.** Notebook 04 reads `Data/Clean/cleaned_movies.csv` directly. Its model feature dataset contains 7,479 positive-revenue rows. The audited TMDB matching work should be described separately unless enrichment into this training file is established.
10. **The README overstates the deployed inputs.** It lists franchise and sentiment among prediction features, but they are deferred and absent from the saved model manifest. The manifest also confirms release quarter was removed.
11. **Power BI and reproducibility packaging remain unverified.** No `.pbix` or `.pbip`, dependency specification (`requirements.txt`, `environment.yml`, `pyproject.toml`) was found in this checkout. Notebook paths are often tied to the original Windows directory. The original plan explicitly includes Power BI. The feature-engineering report still says in progress. A temporary Word lock file is tracked under Reports.
12. **External provenance needs a separate check.** The source register has title, source_field and source columns; many financial entries name outlets rather than linking a dated article. Recorded budget values and released-film outcomes are not independently verified facts merely because they appear in a CSV. Eligibility exclusions and release dates also need traceable evidence before publication.

## Budget investigation

The completed target register has ten supplied budgets and nine unknown budgets. `log1p(budget)` matches the feature file for all 19 rows, including missing values. No budget disappeared in that transformation. The raw relational source contains Shrek 5, Avengers Doomsday and Dune Part Three with zero budgets, which should remain unknown. Older films with similar names have nonzero budgets and must not be substituted for new productions. A title-family search of the two raw sources did not recover a usable standalone budget for the nine future targets; this is not an exhaustive external budget search.

## Recovered future forecast table

Production budgets are unknown in the source register for every row below. Values are model outputs from the committed consolidated export, rounded for reporting.

| Film | Baseline revenue | Baseline blockbuster probability | Hypothetical budget revenue range |
|---|---:|---:|---:|
| Avengers: Doomsday | $59.62 million | 35.61% | $127.28–$1,115.62 million |
| Shrek 5 | $43.61 million | 28.91% | $109.70–$816.58 million |
| Jumanji: Open World | $38.01 million | 43.38% | $57.77–$798.98 million |
| Spider-Man: Beyond the Spider-Verse | $34.97 million | 35.50% | $108.75–$740.99 million |
| Children of Blood and Bone | $16.55 million | 11.73% | $62.20–$630.92 million |
| The Batman Part II | $14.83 million | 10.80% | $48.99–$582.58 million |
| Dune: Part Three | $9.29 million | 7.07% | $45.28–$462.33 million |
| The Cat in the Hat | $7.58 million | 25.11% | $29.92–$636.83 million |
| Street Fighter | $2.18 million | 1.17% | $35.35–$430.72 million |

The ranges cover seven hypothetical budgets from $25 million to $300 million. They are not confidence intervals. The missing-budget baseline uses an effective imputed value of approximately $20 million and a missingness indicator.

## Original research questions and evidence status

| Original question | Audit status | Evidence or remaining requirement |
|---|---|---|
| Which pre-release features are most predictive of revenue? | Partially answered | EDA correlations exist; no permutation-importance, feature_importances_ or partial-dependence analysis found in Notebooks 05/08. Correlation does not establish predictive contribution within the fitted model. |
| How do franchises and sequels compare with original properties? | Deferred | Target-only franchise fields exist; comparable historical training features are absent. Cannot claim an answered comparison. |
| Does release month, season or holiday timing affect performance? | Descriptive evidence present | EDA charts and grouped revenue exist. Clarify descriptive association versus adjusted or causal effects. |
| How strongly do director, cast and company track records relate to revenue? | Descriptive evidence present | Historical correlation and track-record charts exist. Dataset coverage and first-listed entity policy limit interpretation. |
| Does budget exhibit diminishing returns at higher production levels? | Unanswered by existing evidence reviewed | A positive budget correlation and future scenario range do not answer this question. Requires a defined marginal-return analysis or an explicit unresolved status. |
| Can franchise gap capture nostalgia or fatigue? | Deferred | Historical comparable gap features absent from deployed training. |
| Do sentiment and audience-interest signals add predictive value when comparable historical data is available? | Deferred under the stated condition | No comparable historical features or incremental model comparison available. |

The scope in the saved approved plan explicitly includes 2026–2028. A 2028 target is therefore not by itself drift from that saved plan. Earlier conversation scope and the later approved written plan should not be conflated.

## Checks that passed

- Recomputed deployed-family regression and classification metrics agree with the saved evaluation results.
- Saved test labels agree with the training revenue threshold and probability threshold.
- All 81 sensitivity rows have valid scenario classification labels; summary minima, maxima, counts and positive counts agree with recomputation.
- The nine missing-budget scenario baselines reproduce the consolidated prediction values.
- Eight of nine films cross the classification threshold within the seven-budget grid.
- Historical and target CSV feature tables agree with SQLite after normalizing date serialization.
- Source budgets and engineered log budgets align, including missing values.
- Python code cells scanned in repository notebooks have no syntax errors (execution dependencies are a separate matter).

## Additional descriptive evidence recovered

Across available pairs in the historical feature table, Pearson correlations with log revenue are: log budget 0.655; director prior average gross 0.439; company prior average gross 0.411; star prior average gross 0.275. Missing values change the pairwise sample, and these are full-dataset descriptive associations, not validation feature-importance scores.

Median gross is $34.98 million for the stored holiday indicator group versus $18.69 million for the non-holiday group. December median is $43.70 million, June $40.10 million and September $11.67 million. These comparisons are unadjusted for budget, genre, year and selection effects.

## Next actions in dependency order

1. Recover and upload the latest executed upcoming-movie prediction notebook from the local machine. Save it in VS Code first. The repository copy predates the successful fixes and later cells.
2. Declare the active model family explicitly. Preserve the currently used Notebook 09 family for this audit; archive and label the Notebook 08 alternative rather than choosing based on test performance.
3. Restore cohort, budget status, source provenance and complete forecast values in final tables. Correct stale metadata and identify the complete collection notebook in the execution guide.
4. Complete a validation-only interpretation analysis for predictive feature contribution, and decide whether diminishing returns is addressed descriptively or retained as unanswered. Do not tune against the test results.
5. Verify financial-source citations and timestamps before treating the ten released-film cases as a prospective backtest. Compare predictions with supplied outcomes only with appropriate retrospective and provisional labels.
6. Resolve the planned Power BI deliverable as completed elsewhere, outstanding or explicitly removed from scope.
7. Rebuild the final report with the exact seven-question matrix, active model evaluation, both model-family definitions, complete prediction and budget tables, selected EDA and evaluation visuals, source register and clearly stated open limitations.

This audit confirms substantial completed work, several report omissions, and real reproducibility gaps. It does not certify every source claim or the entire pipeline as verified.


## Update after recovery of the complete prediction notebook

The second uploaded notebook contains 45 physical cells and all Sections 01–22, with no saved error outputs. The complete local notebook was therefore recovered; the audited GitHub commit still contains the old incomplete copy.

Corrected the indicator flag assignment before status calculation, removed the outdated indicator limitation, preserved cohort in prediction exports, restricted consolidation to OUTPUT_DIR and its Budget_Sensitivity child, and restored budget, budget_type, budget_source and budget_notes with checked joins. Source budget amounts and engineered log budgets must agree. Distinct result statuses now preserve provisional midpoint and reported-minimum treatment.

Validation: compiled all code cells, executed consolidation, CSV/Markdown export and both chart sections against the committed successful run in an isolated temporary project. All 19 forecast values remained equal to the previous export, nine sensitivity summaries matched, and a second run folder did not affect selection. Notebook display-only Styler expressions were omitted from this check because Jinja2 is not installed in the audit runtime. Full inference was not rerun: saved models specify scikit-learn 1.6.1 and the audit runtime has 1.8.0. The corrected notebook clears old stored outputs and needs Restart Kernel and Run All in the original environment.

Next: replace the local prediction notebook with the corrected version, execute it, then address the remaining model-family documentation, predictive interpretation, diminishing-returns question, source validation and Power BI scope items before report sign-off.


## Model and evaluation audit completed 30 September 2026

The user reports that the corrected prediction notebook runs without errors. Its newly generated outputs have not yet been independently inspected; the checks below use the previously audited repository snapshot.

### Active model family and alternative

Use Notebook 09's `Data/Models/Final` models for the current forecast narrative because the prediction notebook explicitly loads that directory. This is an artifact-lineage decision, not selection based on test performance. Notebook 08's `Models/Final` estimators are a separate train-plus-validation refit and must retain their own results.

| Item | Active Notebook 09 family | Notebook 08 refit alternative |
|---|---|---|
| Training rows | 6,668 | 7,268 |
| Training period | 1980–2015 | 1980–2018 |
| Model directory | Data/Models/Final | Models/Final |
| Result directory | Data/Results/Final_Evaluation | Data/Model_Results/Final |
| Test log RMSE | 2.005540 | 1.934820 |
| Test log R² | 0.542676 | 0.574359 |
| Test classifier accuracy | 0.881517 | 0.872038 |
| Test classifier F1 | 0.725275 | 0.703297 |
| Test classifier ROC AUC | 0.904348 | 0.908944 |

Both families use HistGradientBoostingRegressor and RandomForestClassifier with the recorded selected parameters. Their different training samples explain why the results differ; this is not evidence of a metrics calculation bug. Preserve both until the execution guide clearly distinguishes them. No model relocation, deletion, refit or remote push was performed by this audit.

### Independent checks

- Chronological training: 6,668 rows from 1980–2015; 667 positive labels.
- Validation: 600 rows from 2016–2018; 129 positive labels.
- Test: 211 rows from 2019–2020; 50 positive labels.
- No duplicate title/year keys in the saved split reference. All split reference titles match current feature rows in order.
- Every regression target agrees with log1p of that row's worldwide revenue.
- Every classification target agrees with the training-only 90th percentile cutoff of $183,907,649.60.
- Rebuilt the numeric median-imputation and StandardScaler pipeline, plus categorical most-frequent imputation and one-hot encoding, fitting only training rows. Reconstructed train, validation and test matrices match all stored values to maximum absolute difference below 4e-15. This checks preprocessing independently without fitting revenue or classification models.
- Processed columns agree with the active model manifest; 16 raw predictors become 36 processed columns. Outcome columns are excluded from the predictor list.
- Active and model-ready saved preprocessor files have identical SHA-256 hashes.
- Recomputed classification metrics from saved test predictions agree with the active results JSON. The all-non-blockbuster comparator is 76.30% accuracy versus the model's 88.15%; accuracy alone is insufficient because 17 of 50 positive cases are missed.
- Missing log budget is imputed to a value corresponding to $20 million, reproduced from training-only imputation statistics.
- Notebook 08's probability-threshold selection code uses validation F1, with recall as a tie-breaker, before its test evaluation. The selected probability threshold is 0.57.

### Interpretation boundaries and documentation corrections

The $183.91 million revenue cutoff defines the class; 0.57 is the probability cutoff for assigning that class. They must be labelled separately. Log R² is not dollar-scale R², and classification accuracy is not revenue-prediction accuracy. Active-model revenue MAE is $93.23 million and RMSE is $216.80 million.

Both model configurations have been evaluated on the test set. Describe the actual sequence rather than claiming the test set was evaluated only once across the entire project. No evidence in the inspected code establishes that the test scores were used to tune parameters, but a future model choice must not be made by comparing these test scores.

The feature engineering code allows earlier-year film outcomes in later film histories, including earlier validation/test years. This represents a rolling historical-information setting rather than one frozen forecast made at the training cutoff. Earlier release year alone does not prove a final lifetime gross was available at prediction time; publication/outcome availability is not established by the CSV. These checks therefore do not certify all forms of temporal leakage as absent.

No probability calibration assessment or forecast uncertainty interval has been established. The model artifacts were not deserialized or re-scored here: their saved scikit-learn version is 1.6.1 while this audit runtime is 1.8.0. The independent preprocessing rebuild and saved-prediction checks should not be represented as a full estimator replay.

### NextUp

Complete the research-evidence audit: validation-only model interpretation for feature contribution and a clearly defined descriptive budget diminishing-returns analysis. Keep the three historically unsupported feature families explicitly deferred. Final report revision must include the active-family results above and distinguish implemented, partially answered and deferred questions.

### Active model file fingerprints at the audited commit

- `Data/Models/Final/blockbuster_classifier.joblib`: `22edc4c7cad05f8fcc8367f4b543174d086cd5acc56934ce27791e92ea3efe9a`
- `Data/Models/Final/preprocessor.joblib`: `c58f3dbb90943f9b2beb8fe978e822fa04e7cfba9f5e8d8051c8b318e3a89de2`
- `Data/Models/Final/revenue_regressor.joblib`: `9034680dd0e07a820364cea51b15e8d03c772f390be3d44bc37aff08e5b5f68d`
