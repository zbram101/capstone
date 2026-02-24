# Capstone Problem Statement

## 1. Research Question
Can historical bioprocess sensor data and run metadata be used to predict the likelihood of a quality control (QC) failure before a bioprocess run is completed?

## 2. Expected Data Sources
To develop and validate modeling techniques, this project uses two public Kaggle datasets that are analogous to complex biological read outputs and classification tasks:

1. DNA Classification Dataset  
   Contains structured DNA sample features and classification labels.  
   https://www.kaggle.com/datasets/miadul/dna-classification-dataset
2. DNA Sequence Dataset  
   Contains raw nucleotide sequences from different organisms that can be transformed into predictive features.  
   https://www.kaggle.com/datasets/nageshsingh/dna-sequence-dataset

These datasets support feature engineering and model testing that reflect how predictive models could be applied to real bioprocess telemetry and metadata.

## 3. Techniques Expected in Analysis
The analysis follows a structured pipeline with three methods.

### A. Feature Engineering and Preprocessing
Purpose: convert raw sequence data into numerical features for machine learning models.

- Compute base composition counts: `Num_A`, `Num_T`, `Num_C`, `Num_G`
- Calculate proportions: `GC_Content`, `AT_Content`, `Sequence_Length`
- Generate k-mer frequencies (`k = 3`) to capture short sequence patterns
- Standardize numerical features (zero mean, unit variance) before modeling

### B. Dimensionality Reduction (PCA)
Purpose: reduce dimensionality of high-volume features (especially k-mer vectors) while preserving key variance for downstream classification.

- Apply PCA to the standardized feature matrix
- Select principal components that capture approximately 90% of variance
- Use selected components to reduce noise and improve training efficiency

### C. Predictive Modeling (Classification)
Purpose: train models to estimate QC-like outcomes based on engineered and reduced features.

- Logistic Regression (L2 regularization) as an interpretable baseline
- Random Forest Classifier for nonlinear feature interactions
- Gradient Boosted Trees (for example, XGBoost) for stronger tabular-data performance

Training and evaluation steps:

- Split data into training (80%) and test/validation (20%)
- Use cross-validation and grid search for hyperparameter tuning
- Evaluate with accuracy, precision, recall, F1, and ROC-AUC

## 4. Expected Results
- A predictive classification model that estimates QC failure risk prior to process completion
- Identification of key predictive features (for example, base composition or k-mer patterns)
- Clear visual summaries and interpretable outputs for non-technical stakeholders

## 5. Why This Question Is Important (Non-Technical Explanation)
In bioprocess and life-science operations, quality failures are often discovered only after process completion, which wastes time, materials, and budget. If models can estimate failure risk earlier, teams can intervene sooner by adjusting parameters, rerunning high-risk batches, or reallocating resources. Interpretable outputs help quality engineers and lab managers make faster decisions without requiring machine learning expertise, shifting QC from reactive troubleshooting to proactive risk management.
