# Corrected project report and source review

## Scope and provenance

The supplied 40-slide presentation, four-page group report, 88-cell notebook, and dataset were reviewed for this portfolio edition. The dataset is a CSV stored locally with an `.xls` extension. The slides and report credit **Md Abdul Wahid Raju, Sazzad Hossain, Shusmita Dutta, and Asma Akter**. The presentation labels the interpretation section as presented by Asma Akter. Individual ownership of the code cannot be inferred reliably from the supplied material.

## Data and method

The source contains 7,043 records and 21 columns (19 predictors after excluding `customerID` and `Churn`). `Churn` has 1,869 Yes and 5,174 No records. `TotalCharges` has 11 whitespace-only entries; the notebook converts these to missing numeric values and fills them with zero. The source rows with missing values have tenure zero. The notebook consolidates `No internet service` / `No phone service` with `No`. The shallow-model pipeline uses a stratified 80/20 train/test split (random state 42), five-fold stratified cross-validation for model tuning, and preprocessing inside each pipeline. The network workflow uses a 64/16/20 train/validation/test split, scales using training rows only, and tests four optimizer/learning-rate/activation combinations on validation data.

## Test results from actual evaluation cells

| Model | Accuracy | F1 (churn) | Basis |
| --- | ---: | ---: | --- |
| Soft voting | 0.7807 | 0.6264 | Notebook cell 38 |
| Random forest | 0.7701 | 0.6241 | Notebook cells 31, 34 |
| Logistic regression | 0.7395 | 0.6125 | Notebook cells 29, 34 |
| Feedforward neural network | **0.7913** | **0.6111** | Notebook cell 60 |
| Gradient boosting | 0.8027 | 0.5813 | Notebook cells 33, 34 |
| Autoencoder + logistic regression | **0.6828** | **0.5377** | Notebook cell 68 |

**Correction:** The original notebook's cell 69 manually entered 0.7963/0.6008 for the FFNN and 0.7232/0.5963 for the autoencoder classifier. The report and slide comparison repeat these values, but they disagree with the notebook's actual test evaluation (cells 60 and 68). A preliminary concatenation in cell 72 also mixed validation experiment records with test results. The portfolio notebook now builds the comparison from evaluated variables and keeps validation results separate. The edited cells' cached outputs and the rest of the historical outputs were removed. Since no complete end-to-end rerun was performed here, results in this report describe the supplied notebook's saved run.

## Interpretation and limitations

The voting confusion matrix is `[[841, 194], [115, 259]]` (actual classes as rows, predicted classes as columns), yielding churn precision 0.5717, recall 0.6925, and F1 0.6264. The forest's F1 is 0.6241, only 0.0022 below voting. A held-out score alone does not validate a production deployment choice, calibration, net economic benefit, or stable subgroup performance.

The forest SHAP figure is a dot/beeswarm explanation for the churn class, computed on 200 sampled test rows of 39 preprocessed columns. The NN uses 200 standardized rows with 23 encoded features. Red/blue correspond to high/low **encoded input values**; SHAP units and preprocessing differ between these models, so numerical attribution magnitudes should not be compared directly. Correlated contract, tenure and charge variables complicate causal interpretations. Both figures come from the source notebook's embedded plot outputs; no new SHAP estimates were fabricated.

The original report calls 21 columns “21 features”; more precisely there are 19 raw model predictors. Its recommendation to deploy the ensemble is stronger than the evidence supports. The data are an IBM fictional sample, with no real-world intervention or temporal validation. Some original code relies on notebook cell order and produces a cleaned CSV in the current working directory. The updated notebook removes hard-coded comparison numbers and uses a relative data path, but its complete execution and dependency compatibility remain unverified.

## Portfolio use

Cite the project as group coursework, credit all four members, and describe your own contribution only to the extent you can substantiate. Present the voting result and genuine SHAP beeswarm as demonstrations of classification and interpretability. Avoid saying the analysis proves why customers leave or guarantees an effective retention strategy.
