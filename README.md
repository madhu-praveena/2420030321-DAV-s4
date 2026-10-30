# Customer Churn Prediction

**Engineering Capstone Project – 1 | Data Analytics and Visualization (DAV)**  
KL University · Department of Computer Science & Engineering  
Team: S. Vaishnavi · Madhu · Hema  
Project Guide: Dr. Shivashri · Academic Year: 2026–2027

## Project overview

Customer churn prediction is a binary-classification problem: use customer profile, account, service and billing attributes to estimate whether a customer is likely to leave. This repository provides a reproducible data-analysis and machine-learning workflow for the IBM Telco Customer Churn sample dataset.

The pipeline covers:

1. Dataset acquisition and schema validation
2. Data understanding, missing-value and duplicate checks
3. Exploratory analysis and statistical checks
4. Preprocessing with imputation, one-hot encoding and model-appropriate scaling
5. Stratified train/test split
6. Comparison of Logistic Regression, Decision Tree, Random Forest, SVM, KNN and XGBoost (when installed)
7. Evaluation using accuracy, precision, recall, F1-score and ROC-AUC
8. Saving the selected model and reproducible outputs

> **Results integrity:** The repository initially contains no measured model scores. Run the experiment locally to generate `results/model_metrics.csv` and the plots. Do not copy the illustrative values from `frontend-demo/index.html` into academic results; that page is a UI prototype with demo scoring, not a trained model.

## Dataset

We use IBM's public **Telco Customer Churn** sample dataset: 7,043 customer records and 21 columns. IBM describes it as a sample for a fictional telecommunications company; `Churn` indicates whether the customer departed within the last month.

- Official IBM documentation: https://www.ibm.com/docs/en/cognos-analytics/12.1.x?topic=samples-telco-customer-churn
- Original IBM repository: https://github.com/IBM/telco-customer-churn-on-icp4d
- CSV download: https://raw.githubusercontent.com/IBM/telco-customer-churn-on-icp4d/master/data/Telco-Customer-Churn.csv
- Dataset details and column guide: [`docs/data_dictionary.md`](docs/data_dictionary.md)

The CSV is downloaded by script and is not committed to Git, keeping the repository lightweight. Review the source repository's license and terms before reuse.

## Quick start

### 1. Create an environment

```bash
python -m venv .venv
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux:
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 2. Download the dataset

```bash
python -m src.download_data
```

This saves the original CSV to `data/raw/Telco-Customer-Churn.csv`.

### 3. Train and evaluate all models

```bash
python -m src.train
```

Generated artifacts:

- `results/data_quality.json` — dataset dimensions, missing values, duplicates and class balance
- `results/model_metrics.csv` — measured test-set metrics for each available classifier
- `results/classification_reports.json` — per-class precision/recall/F1
- `results/roc_curves.png` — ROC curves
- `results/churn_distribution.png` — target-class distribution
- `models/best_model.joblib` — selected fitted pipeline
- `results/run_metadata.json` — split, seed, selected model and run details

### 4. Reproduce the run

The experiment uses `random_state=42` and a stratified 80/20 train/test split. The test set is held out from model fitting. Preprocessing is fitted inside each model pipeline using training data only.

## Project structure

```text
Customer-Churn-Prediction/
├── data/
│   ├── raw/                  # downloaded CSV (not committed)
│   └── README.md
├── docs/
│   ├── data_dictionary.md
│   └── methodology.md
├── frontend-demo/
│   ├── index.html            # existing UI prototype
│   └── README-original.md
├── models/                   # trained model output (not committed)
├── notebooks/
│   └── Customer_Churn_Analysis.ipynb
├── results/                  # generated metrics/plots (not committed)
├── src/
│   ├── __init__.py
│   ├── download_data.py
│   └── train.py
├── tests/
│   └── test_data_download.py
├── .gitignore
├── Makefile
└── requirements.txt
```

## Methodology

- **Target:** `Churn` (`Yes` = 1, `No` = 0).
- **Identifier:** `customerID` is excluded from predictors.
- **Data cleaning:** `TotalCharges` is converted to numeric; blank strings become missing and are imputed within the pipeline.
- **Categorical features:** most-frequent imputation followed by one-hot encoding.
- **Numeric features:** median imputation; standardisation for Logistic Regression, SVM and KNN. Tree-based models do not require standardisation.
- **Model comparison:** same stratified split and metrics for all models.
- **Selection:** F1-score is the primary comparison criterion, with recall and ROC-AUC used as tie-breakers and interpreted in the business context.

See [`docs/methodology.md`](docs/methodology.md) for details and limitations.

## Existing front-end prototype

`frontend-demo/index.html` is the interactive ChurnIQ UI supplied with the original project ZIP. Its prediction score is a transparent demo rule and its displayed KPI/model metrics are illustrative placeholders. It is not connected to the trained pipeline. The backend/API integration is a future extension.

## Limitations

- The IBM data is a cross-sectional sample, not a time-series dataset. The experiment is a held-out classification evaluation, not a guarantee of future-month performance.
- Relationships in the data do not prove causation.
- Metrics depend on the split, preprocessing, hyperparameters and class balance.
- A prediction should support, not replace, a retention team's judgement.

## Team

S. Vaishnavi · Madhu · Hema  
Guide: Dr. Shivashri  
KL University · A.Y. 2026–2027
