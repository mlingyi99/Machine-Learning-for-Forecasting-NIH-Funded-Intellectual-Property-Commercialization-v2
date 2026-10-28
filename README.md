# Machine Learning for Identifying Patent-Linked NIH-Funded Research

## Overview

This repository contains the data-processing, machine-learning, evaluation, and interpretation code supporting the manuscript:

**“Machine Learning for Identifying Patent-Linked NIH-Funded Research: Evidence from NCI Awards in Pennsylvania.”**

The study examines whether characteristics of NIH/NCI-funded research projects can be used to identify projects associated with USPTO patent records.

The repository is provided to support transparency, reproducibility, and editorial and peer-review assessment.

## Repository Structure

```text
NIH-Patent-ML/
├── README.md
├── requirements.txt
├── Manuscript_Code.ipynb
├── data/
│   ├── Patents.csv
│   └── All_Last_10_NCI_pa.csv
└── results/
```

### `Manuscript_Code.ipynb`

The Jupyter Notebook contains the complete analytical workflow, including:

1. Importing required Python libraries
2. Loading NIH/NCI grant and patent data
3. Linking grant records with patent records
4. Constructing the patent-linked outcome
5. Data cleaning
6. TF-IDF text feature engineering
7. Combining text and structured features
8. Train/test splitting
9. Machine-learning model training
10. Model evaluation
11. Confusion-matrix generation
12. SHAP-based model interpretation
13. Historical patent-rate analysis
14. Exporting model-performance results

## Data

### NIH/NCI Grant Data

`All_Last_10_NCI_pa.csv`

This dataset contains the NIH/NCI research-project information used in the analysis.

Relevant fields used by the analysis include:

- `core_project_num`
- `project_title`
- `abstract_text`
- `award_amount`
- `fiscal_year`

### Patent Data

`Patents.csv`

This dataset contains patent records used to identify NIH/NCI projects associated with at least one patent.

The datasets are linked using:

```text
NIH/NCI: core_project_num
USPTO/patent data: PROJECT_ID
```

Patent records are aggregated by `PROJECT_ID` to calculate the number of associated patents for each research project.

## Outcome Definition

The binary outcome variable `Commercialization` is constructed from the patent linkage.

A project is assigned:

```text
Commercialization = 1
```

if it is associated with at least one patent record, and:

```text
Commercialization = 0
```

if no associated patent record is identified.

The variable is implemented as:

```python
df["Commercialization"] = (df["patent_count"] > 0).astype(int)
```

In the context of the analysis, this variable represents **patent association**. It should not be interpreted as a comprehensive measure of all forms of research commercialization.

## Features

### Text Features

Text is constructed by combining the project title and abstract:

```python
project_title + abstract_text
```

TF-IDF vectorization is then applied with:

- Maximum features: 5,000
- English stop-word removal

### Structured Features

The structured predictors used in the modeling analysis are:

- `award_amount`
- `fiscal_year`

Other potentially leakage-prone variables are excluded from the modeling feature set.

## Machine-Learning Models

Four classification algorithms are evaluated:

- Logistic Regression
- Random Forest
- Gradient Boosting
- Multinomial Naive Bayes

The dataset is divided into training and testing subsets using an 80/20 stratified split with `random_state=42`.

Models are compared using:

- Accuracy
- F1 score
- ROC-AUC

The model with the highest test-set F1 score is selected for subsequent interpretation.

## Model Interpretation

SHAP is used to examine feature contributions for the selected model.

Depending on the selected estimator, the notebook uses either a tree-based SHAP explainer or a model-agnostic SHAP approach.

The analysis produces SHAP feature-importance and feature-impact visualizations.

## Historical Analysis

The notebook also calculates the observed proportion of patent-linked projects by fiscal year.

This analysis is descriptive and does not perform forecasting.

## Outputs

Running the notebook generates model-evaluation results and manuscript-related figures, including:

- Patent-label distribution
- Confusion matrix
- SHAP feature-importance plot
- SHAP summary plot
- Historical patent-rate trend
- Model-comparison results

Generated outputs should be stored in the `results/` directory.

## Installation

Python 3 is required.

Install the required packages using:

```bash
pip install -r requirements.txt
```

## Running the Analysis

1. Download or clone the repository.
2. Install the dependencies listed in `requirements.txt`.
3. Place the required input datasets in the `data/` directory.
4. Open `Manuscript_Code.ipynb`.
5. Run the notebook cells sequentially from beginning to end.

The analysis uses fixed random seeds where applicable to improve reproducibility.

## Data Sources and Availability

The research data used in this study were derived from publicly available NIH/NCI and USPTO-related sources.

The repository contains the analytical materials necessary to understand the data-processing, patent-linkage, feature-engineering, machine-learning, evaluation, and interpretation procedures used in the manuscript.

Please refer to the manuscript's Data Availability statement for additional information regarding the underlying data and access conditions.

## Reproducibility

The repository is intended to provide sufficient documentation and analytical code for editors, reviewers, and researchers to examine and reproduce the analyses reported in the manuscript.

Software dependencies are documented in `requirements.txt`.
