# Capstone Project: Predicting QC Failure Risk from Sequence-Derived Features

## Project Overview

This project builds a DNA sequence classification pipeline using public sequence datasets, with a focus on end-to-end workflow quality: cleaning, feature engineering, exploratory analysis, and baseline modeling.

## Research Question

How accurately can engineered DNA sequence features predict class labels, and which sequence-derived signals (base composition and k-mer patterns) are most informative?

## Data Sources

This project uses two public Kaggle datasets as analogs for bioprocess telemetry and metadata:

1. DNA Classification Dataset  
   https://www.kaggle.com/datasets/miadul/dna-classification-dataset
2. DNA Sequence Dataset  
   https://www.kaggle.com/datasets/nageshsingh/dna-sequence-dataset

## Methodology

### 1. Data Preparation

- Merge and standardize schema across both datasets as needed.
- Handle missing values, duplicate rows, and malformed sequences.
- Encode target labels for binary/multiclass classification.

### 2. Feature Engineering

- Base-count features: `Num_A`, `Num_T`, `Num_C`, `Num_G`
- Composition features: `GC_Content`, `AT_Content`, `Sequence_Length`
- k-mer features (k=3) using normalized frequencies
- Standardization of numeric features for linear models and PCA

### 3. Exploratory Data Analysis (EDA)

- Class distribution plots
- Sequence length and base composition distributions
- Correlation heatmap for engineered tabular features
- PCA variance analysis and 2D projection colored by class

### 4. Baseline Modeling

- Baseline model: Logistic Regression (L2)
- Train/test split: 80/20 (stratified)
- Metrics: Accuracy, Precision, Recall, F1, ROC-AUC

### 5. Planned Model Expansion

- Random Forest
- Gradient Boosted Trees (XGBoost if environment supports it)
- Hyperparameter tuning and comparison against baseline

## Results

### Data Quality and Cleaning Summary

- Sequences were standardized to uppercase and whitespace-trimmed.
- Rows with missing `sequence`/target were removed and duplicate rows were dropped.
- Non-DNA rows were filtered using `^[ATCG]+$` to keep valid nucleotide strings only.
- Modeling matrix was additionally guarded against all-NaN feature rows and missing targets before split.
- Cleaned modeling dataset shape: `5708 x 2` (`sequence`, `class`)
- Engineered feature matrix shape: `5708 x 72`
- Any missing target after feature assembly: `False`

### EDA Findings

- Target classes are multiclass and show visible imbalance (captured in class-count plot in notebook).
- Cleaned class distribution:
  - `6.0`: `1753`
  - `4.0`: `898`
  - `3.0`: `831`
  - `0.0`: `769`
  - `1.0`: `633`
  - `2.0`: `467`
  - `5.0`: `357`
- Sequence-length distributions vary by class and are separable in parts of the range.
- Engineered base-count/composition features (`Num_*`, `GC_Content`, `AT_Content`, `Sequence_Length`) show interpretable correlation structure.
- Added explicit IQR-based outlier analysis (table + boxplot) for key engineered numeric features.
- PCA summary from rerun: components for >=90% variance: `30`; cumulative explained variance at component 30: `0.9039`.

### Baseline Model Performance

- Model: Logistic Regression (L2)
- Primary metric: Weighted F1 (chosen for class imbalance robustness)
- Most recent run metrics:
  - Test Accuracy: `0.6138`
  - Weighted Precision: `0.6066`
  - Weighted Recall: `0.6138`
  - Weighted F1: `0.6064`

### Initial Interpretation

- The baseline indicates moderate predictive signal from sequence-derived engineered features, but there is clear room for improvement in minority-class behavior.
- Class imbalance and overlap between classes likely limit linear-separable performance; this motivates tree-based nonlinear models and tuning.

## Repository Structure

- `Problem_Statement.md`: Initial project framing and expected methodology
- `README.md`: Project narrative and results updates
- `notebooks/module23_eda_baseline.ipynb`: EDA and baseline model workflow
- `data/raw/`: Source files (local, not committed if large)
- `data/processed/`: Cleaned/intermediate datasets

## How to Run

1. Install dependencies including `kagglehub` (for example: `pip install kagglehub pandas numpy seaborn matplotlib scikit-learn`).
2. Ensure Kaggle credentials are configured in your environment.
3. Open `notebooks/module23_eda_baseline.ipynb`.
4. Run all notebook cells; the notebook downloads both datasets using `kagglehub.dataset_download(...)`.
5. If `kagglehub` is unavailable, use the manual local path fallback in the first setup cell.
