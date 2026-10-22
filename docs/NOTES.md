# Methodology, provenance and publication notes

Prepared 22 October 2026. This is a historical portfolio adaptation; no training, prediction or notebook execution was performed.

## Technical interpretation

1. **Objective and gradient.** The intended mathematical objective is C times the summed hinge losses plus one half of the feature-weight squared norm, with unpenalised bias. The retained batch code accumulates `-y*x` for margin-violating examples and adds `2*c*w` to all augmented weights, including bias. Away from hinge kinks this corresponds instead to summed hinge loss plus c times the augmented-weight squared norm. Logged `cost` contains only summed hinge loss; it is measured before the update whereas stored weights are after it. The preserved plot labels must not be interpreted as the full specified objective.
2. **Report derivation.** The historical report shows a per-example expression without the active-example sum needed for its full objective. Its expansion of subtracting the gradient changes the regularisation sign incorrectly. No corrected derivation is substituted into the historical code.
3. **Classification preprocessing.** The training imputer/scaler are fitted on training data, but the evaluation code then calls `fit_transform` on test data, overwriting them and using a different coordinate system. Historical test metrics are retained with that limitation. Reusing training transforms would be the appropriate correction in a separate study, not a change made here.
4. **Initial CV leakage.** The architecture search standardises/imputes the whole training split before five-fold GridSearchCV. Each CV validation fold therefore influences preprocessing. The held-out regression test split is transformed with training-fitted objects. The earlier robust-pipeline search puts preprocessing inside GridSearchCV; the final notebook directly fits that selected pipeline.
5. **Uncertainty.** The final source prints 0.01668723115841049 as a standard deviation. Its expression computes the standard deviation of mean validation scores across candidate models. It is neither fold uncertainty for the best model nor repeated-run variability. The earlier source prints no such standard deviation. The CSV labels this quantity explicitly.
6. **Learning-rate conclusion.** The report favours 0.001 as highest in both accuracy and F1. In the earlier saved table, highest accuracy is at 0.001 but highest F1 is at 0.01. In the final table both are highest at 0.0001. Different unseeded initialisations, a non-fixed classification split and one run per setting prevent a firm optimum claim. Shared baseline outputs across versions are not independent validation.
7. **Optimisers and robustness.** Both 300-iteration fits emit maximum-iteration convergence warnings. Only training-loss histories and final training/testing MSE were preserved. Loss-curve values are scikit-learn's training objective, not test MSE. No per-iteration test-MSE series, controlled noise experiments or additional missingness tests were recovered. RobustScaler supports the preprocessing design but does not prove robustness.
8. **Evaluation scope.** Regression uses an 80/20 split with random_state=42; classification uses a stratified 85/15 split without a fixed seed. Repeated inspection of the same test split during experiments limits its independence. Random splits of hourly measurements do not validate forecasting or future sensor-drift performance. Metrics are historical observations, not coursework marks or official unseen-test scores. The model-loading demonstration and its arbitrary three-example score are excluded.

## Source records and transformations

“Final” denotes the completed notebook modified 7 May 2024, with sequential execution counts and selected-pipeline fitting. “Earlier” denotes the larger experimental notebook with pipeline grid search and verbose optimiser logs. Actual submission is unconfirmed. The report is dated 7 May 2024; its two recovered local copies are byte-identical. The notebook checkpoint agreeing with the final source supports selection but is not a separate experimental run.

Original source fingerprints (SHA-256; paths and student usernames intentionally omitted):

| Reference | SHA-256 |
|---|---|
| final notebook | `2d1978e937a1c53a74c5d99c5ca6a5e38619462d9227fa2a9d6a044bfaba56d4` |
| earlier notebook | `8f6c66d7ca7fef4946a9b098751c110f9b6e92b611486327defa3c8aa5ca0dff` |
| starter notebook | `e18cd47a4c42e879d7feb12ce5a00b4697ae2194be653107b788c0cde8982d59` |
| coursework workbook | `f5971a9c21ef30f0cd7e89cb078ffb2513126bf2b675814ecc9d6ea84393c104` |
| historical report | `5eca425857af4786b16f6cd1264949faff03dbbbdea26e3255477a87a5e044bc` |

Executable cells are from the final notebook; original indices below are zero-based. The earlier pipeline-search cell 23 is included only as a fenced, non-executable appendix. The supplied duplicate optimiser-parameter cell, timer output, model persistence and example-data loading check were omitted.

| Public code-cell index | Original final cell index |
|---|---|
| 2 | 2 |
| 4 | 4 |
| 6 | 6 |
| 8 | 8 |
| 10 | 10 |
| 12 | 13 |
| 14 | 15 |
| 16 | 19 |
| 18 | 23 |

All original assessment markdown/raw instructions were replaced with new explanatory prose. Placeholder comments/docstring text were removed. Both workbook reads now use DATA_PATH; a small pathlib resolver supports kernels started in the root or notebooks directory. The exploratory sample call and its raw five-row output were removed. Stored execution counts and cell metadata were cleared. Historical warning paths were replaced with a neutral environment label. Three PNG outputs are preserved byte-for-byte. All other retained code computations and numeric output tokens are unchanged. The appendix has the same path/comment sanitisation.

The CSV transcribes exact printed numeric tokens from each source version, with source cell indices. It includes both copies of matching regression metrics to preserve source provenance, not to claim two independent runs. README regression values are rounded for display. The original PDFs, workbook, fitted pickle, helper module, starter notebooks/packages and sample rows are absent.

## Publication and upstream review

The current [University of Manchester guidance](https://www.staffnet.manchester.ac.uk/adviceandresponse/resources/types-of-potential-misbehaviour/) distinguishes a student's own assessment content, usable after the examination period, from university questions, instructions and feedback. It also restricts sharing university intellectual property without permission. This is current guidance checked on 22 October 2026, not a recovered 2024 COMP24112-specific policy or individual permission. No course-specific publication prohibition was found in the available source material; separate forum or submission instructions were not recovered.

### Final scaffold and brief review (22 October 2026)

The complete 30-cell starter notebook serves as the available coursework brief; no separate COMP24112 brief or forum/submission-policy document was recovered. It contains no explicit restriction on public sharing of completed work and no identifiable copyright header or licence grant. Its restriction on scikit-learn in the linear-classifier implementation is an assessment constraint, not a publication rule. University-wide guidance does restrict sharing university intellectual property and distinguishes that from a student's own completed assessment content.

Direct comparison identifies the following retained teaching-origin material:

| Portion | Assessment |
|---|---|
| Function signatures, argument descriptions, random-generator setup and return skeleton | Ordinary interface/organisation scaffolding. The starter leaves design matrices, initial weights, costs, gradient updates and predictions as placeholders. The completed classifier implementation was added beyond those placeholders. |
| Imports, workbook loading and initial classification preprocessing | Conventional pandas/NumPy/scikit-learn calls: threshold a target, drop its column, stratify a split, impute means and standardise features. This is functional teaching code, but contains no bespoke preprocessing algorithm or substantial reusable university implementation. Its origin remains credited. |
| Architecture grid and optimiser settings | Small dictionaries of prescribed model choices and standard library options, not implementations of MLPs, SGD or Adam. Experiment orchestration was added around them. |
| Persistence helper and example-data check | Excluded, together with all assessment questions, rubrics and full teaching notebooks. No university-provided completed learning algorithm or evaluation framework is redistributed. |

**Review conclusion: public GitHub publication of this sanitised package is reasonable; no material publication blocker was identified.** The retained overlap is ordinary API/configuration scaffolding rather than substantive university algorithm implementation. This is an evidence-based publication-readiness assessment, not a declaration that all scaffolding is uncopyrightable, a university permission grant or a new licence. No course-specific public-sharing prohibition was found in the available brief. Unrecovered forum instructions remain a scope limitation, not an assumed prohibition. Any additional restriction actually known to the author would require reassessment.

Keep the existing university/source attribution and any applicable upstream notices. No university author or licence is invented, and no repository-wide LICENSE is assigned. The dataset, saved model, original report and starter package remain excluded; unresolved rights to redistribute those excluded files do not block publication of this seven-file package.

The university supplied the problem, dataset, function skeletons, initial classification preprocessing, architecture grid and optimiser settings. The student contribution consists of implementation added to those skeletons, evaluation and experiment orchestration. A sole-author report, username-based artifacts and local development history support attribution; an explicit individual/group declaration and submission receipt were not recovered. Recovery does not establish sole authorship of every line. An earlier report revision contains assistant-style explanatory passages; the exact tool and extent are unverified. The report is reference-only and is not redistributed.

NumPy, pandas and Matplotlib provide array, table and plotting functionality. scikit-learn supplies the estimators, optimisers, preprocessing, cross-validation and regression metrics. Its historical-version [licence](https://github.com/scikit-learn/scikit-learn/blob/1.2.2/COPYING) is BSD 3-Clause; source/binary redistribution requires retaining its notices, conditions and disclaimer, and its names must not imply endorsement. This package bundles no dependency implementation; dependency licences remain with the installed distributions and do not license the student's coursework. No additional identifiable copied tutorial, dependency source or third-party licence header was found in the compared coursework cells. That is not proof of every line's origin. The [dataset notes](../data/README.md) describe unresolved subset provenance and terms.

## Privacy and validation

Public copies omit personal university usernames, machine-specific absolute paths, notebook metadata, raw dataset examples, fitted models and original report metadata. Only professional author attribution remains. Ignore rules exclude common data/model/archive formats and local environments; they are an additional safeguard, not publication permission.

Final review rechecked all seven files: notebook v4.5 structure, unique cell IDs, output fields, PNG integrity/metadata, internal file links and privacy scans pass. No student IDs, credentials, private paths, assessment questions, HTML data tables or embedded dataset rows were found; the only images are the three previously visually reviewed plots. Completed validation: all nine retained code cells compile without execution and match the source computations after the documented non-methodological changes; the archived earlier cell also matches. All 43 CSV metric tokens match saved source outputs. The three original PNGs match byte-for-byte. Privacy/assessment-text scans, internal file links, the seven-file package inventory and original-source hashes pass. No dependency installation, model loading, end-to-end run or numerical reproduction is claimed. Dependency versions other than the historical clues are not established.
