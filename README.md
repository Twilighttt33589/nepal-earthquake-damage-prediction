# Nepal Earthquake Damage Prediction

This repository contains my data-preparation and ensemble-modeling work for DrivenData's **Richter's Predictor: Modeling Earthquake Damage** practice competition. The task is to predict an ordinal building-damage grade (1, 2, or 3) from pre-earthquake building, household, and location characteristics.

## Results

- Fixed validation Micro F1: **0.75589**
- DrivenData public Micro F1: **0.7515**
- Public leaderboard rank at submission: **252**

The final approach blends predicted class probabilities from CatBoost and one-hot encoded LightGBM, using a validation-selected weight of 60% CatBoost and 40% LightGBM.

## Repository scope

This personal repository includes the parts I developed:

- `01_Preprocessing_EDA.ipynb` — data validation, exploratory analysis, core feature engineering, and the fixed stratified split.
- `02_Supplementary_Preprocessing_Geography_EDA.ipynb` — supplementary structural and geography-aware feature engineering.
- `04_CatBoost_LightGBM_Tuning_Ensemble.ipynb` — CatBoost and LightGBM tuning, probability blending, error analysis, and submission generation.

It intentionally does **not** include independently authored team notebooks.

## Data access

The competition data is **not included** in this repository. DrivenData's competition rules prohibit participants from redistributing the data publicly. After joining the competition, download the four CSV files from the official data page and place them in:

`Preprocess & EDA/raw_data/`

Required files:

```text
train_values.csv
train_labels.csv
test_values.csv
submission_format.csv
```

Competition page: https://www.drivendata.org/competitions/57/nepal-earthquake/

## Setup

```bash
python -m pip install -r requirements.txt
```

Use a Python 3.12 environment. CatBoost and LightGBM are required by notebook 04. The first cell of notebook 04 checks for these packages as an additional safeguard.

## Reproduction order

Run the notebooks in this order from the repository root:

1. `Preprocess & EDA/01_Preprocessing_EDA.ipynb`
2. `Preprocess & EDA/02_Supplementary_Preprocessing_Geography_EDA.ipynb`
3. `Modeling/04_CatBoost_LightGBM_Tuning_Ensemble.ipynb`

Notebooks 01 and 02 create local processed-data directories. Notebook 04 reads those generated artifacts, selects the strongest validation solution, retrains it on all labelled data, and writes a submission CSV under `Modeling/results_04_tuning/`.

## Method highlights

- Retain `building_id` for traceability and final submission, but exclude it from training features.
- Treat anonymous construction codes as nominal categorical variables rather than ordered quantities.
- Build interpretable structural features such as building age flags, height per floor, families per floor, material counts, and reinforced-concrete indicators.
- Use a fixed stratified validation split and avoid target-derived geographic features to prevent leakage.
- Compare complementary categorical-data approaches: LightGBM after one-hot encoding and CatBoost with native categorical features.
- Select the final probability blend exclusively on the fixed validation set.

## License

Code in this repository is released under the [MIT License](LICENSE). The dataset remains subject to DrivenData's competition terms.
