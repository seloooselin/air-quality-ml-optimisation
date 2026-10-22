# Air Quality ML

**Gradient Descent, Classification and Neural Network Regression**  
Selin Deniz İşken · University of Manchester · COMP24112 Machine Learning · 2024

A compact study of carbon-monoxide prediction from multisensor measurements: a NumPy linear classifier, learning-rate experiments and scikit-learn neural-network regression. This portfolio preserves historical code, plots and recorded results, with its methodological errors documented rather than silently repaired.

## Explore the work

Open [the notebook](notebooks/air_quality_ml.ipynb) to inspect the implementation and three saved figures without training. [Recorded metrics](results/recorded_metrics.csv) retain exact printed precision and source-version labels; [notes](docs/NOTES.md) explain limitations, provenance and publication review. No data, fitted models or assessment documents are bundled.

## Implementation

The coursework workbook has 3,304 observations, 11 predictors and a CO(GT) target. Missing features use -999. The binary task labels CO(GT) > 4.5 positive; the regression task predicts CO directly. Only 224 observations are positive, so accuracy can conceal poor minority-class detection.

- **Custom code added to teaching scaffolding:** design-matrix construction, margin checks, accumulated batch gradient updates, prediction, manual accuracy/precision/recall/F1 calculations and learning-rate experiment orchestration. The loop uses NumPy operations but is not fully vectorised.
- **scikit-learn functionality:** data splitting, mean imputation, standard/robust scaling, MLPRegressor, GridSearchCV and regression metrics. Neural networks, backpropagation, SGD and Adam are library implementations.
- **Model selection:** the initial five-fold search compares (3,), (100,), (3,3), (100,100) with ReLU/logistic activation. An earlier pipeline search varies architecture, activation, alpha and solver; its code appears as a non-executable appendix. The final notebook fits the selected pipeline directly.

## Recorded results

These are saved historical outputs, not fresh reproduction results or official unseen-test scores. “Final” identifies the final-style recovered notebook, not a verified submission receipt. Versions are separate source records, not independent repeated trials.

The baseline classifier records accuracy **0.9597** and positive-class F1 **0.6296** in both versions. The learning-rate runs differ:

| Learning rate | Earlier accuracy | Earlier F1 | Final accuracy | Final F1 |
|---|---:|---:|---:|---:|
| 0.1 | 0.8911 | 0.5574 | 0.9194 | 0.0000 |
| 0.01 | 0.9577 | 0.7586 | 0.9456 | 0.7158 |
| 0.001 | 0.9637 | 0.6667 | 0.9617 | 0.6545 |
| 0.0001 | 0.9577 | 0.6182 | 0.9677 | 0.7241 |

Accuracy counts all correct labels; F1 balances precision and recall for the rarer high-CO class. The final 0.1 run illustrates why both matter. Unseeded initialisation and single runs limit attribution of differences to learning rate.

Both source notebooks record the following regression values (rounded here; exact tokens are in the CSV):

| Model | Train MSE | Test MSE | Test R² |
|---|---:|---:|---:|
| Selected ReLU MLP, (100,) | — | 0.192264 | 0.909922 |
| Adam, (100,100), 300 iterations | 0.069414 | 0.211831 | 0.900755 |
| SGD, (100,100), 300 iterations | 0.150279 | 0.192380 | 0.909868 |
| Mean imputation → RobustScaler → selected MLP | — | 0.190326 | 0.910830 |

The initial architecture search records CV MSE **0.170489**. The selected pipeline uses ReLU, (100,), Adam and alpha 0.001. Adam achieves lower training MSE in the comparison, while SGD has lower test MSE in this recorded run. No statistical significance or general superiority is established.

## Methodological limits

- The custom update differs from the intended objective: `c` weights regularisation rather than hinge loss, the bias is penalised, and logged “cost” omits regularisation.
- Classification refits imputation and scaling on test features. Initial MLP preprocessing occurs before cross-validation, leaking fold information. The later pipeline search includes preprocessing inside CV.
- The printed standard deviation is spread across candidate mean CV scores, not selected-model fold uncertainty. The original report also contains gradient algebra errors and a learning-rate conclusion contradicted by the saved tables.
- Splits are random, not temporal; classification splits and weights are unseeded. The test set is inspected across configurations. These results do not establish future-time generalisation or an untouched final benchmark.
- Both 300-iteration optimiser runs record convergence warnings. Only training-loss curves and final MSE are available. Robust scaling was used, but controlled noise/missingness robustness tests were not recovered.

See [the notes](docs/NOTES.md) for precise boundaries. Historical computations remain unchanged.

## Optional local execution

Browsing saved outputs needs no setup. Execution requires an authorised copy of the exact coursework workbook; follow [data instructions](data/README.md). The UCI download is not a verified substitute.

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter notebook notebooks/air_quality_ml.ipynb
```

Start the kernel in the package root or notebooks/ and run cells in order only if intentionally conducting a new historical-code run. It will train models and retains the documented errors. Save new outputs separately. Python 3.11.7 and scikit-learn 1.2.2 are historical clues, not a complete environment lock; current dependency compatibility and numerical reproduction are unverified. No notebook was executed during preparation.

## Attribution

The university supplied the task, function skeletons, initial classification preprocessing and prescribed search/comparison settings. Student additions implement the classifier and experimental workflows. Recovery supports a sole-author coursework attribution but does not conclusively establish individual/group rules or authorship of every line. An earlier report draft contains assistant-style explanatory text; its tool and extent are unverified. Portfolio documentation and sanitisation were prepared with Codex assistance.

The [final publication review](docs/NOTES.md#publication-and-upstream-review) supports public GitHub publication of this sanitised historical package: no material publication blocker was identified in the reviewed evidence. Retained teaching material is ordinary API/configuration scaffolding, credited above; the available brief contains no explicit public-sharing restriction. This assessment does not grant university permission or resolve dataset redistribution rights. No repository-wide reuse licence is assigned. No GitHub repository has been created by this preparation.
