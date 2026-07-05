# Mental Health in Tech — Predicting Who Seeks Treatment

Predicting whether a tech-industry employee has sought mental health
treatment, using real survey data — and uncovering which workplace
factors actually drive that decision.

## Overview

| | |
|---|---|
| **Dataset** | [OSMI Mental Health in Tech Survey (2014)](https://www.kaggle.com/datasets/osmi/mental-health-in-tech-survey) |
| **Size** | 1,259 responses, 27 questions |
| **Task** | Binary classification — will this employee seek treatment? |
| **Models** | Logistic Regression, KNN, Decision Tree, Random Forest |
| **Best result** | Random Forest — 84.9% accuracy, 89% recall |

## Motivation

Mental health support in the workplace is under-discussed, especially in
high-pressure industries like tech. This project asks two questions:

1. Can we predict whether an employee is likely to seek treatment, based
   on workplace conditions?
2. What actually drives that decision — company policy, personal history,
   or something else?

Beyond the analysis itself, this project is meant to demonstrate a
complete, honest data science workflow: cleaning real messy data,
comparing models fairly, and explaining why the best model works — not
just reporting an accuracy number.

## The Data Wasn't Clean — Here's What Had to Be Fixed

This is real survey data, not a tidy Kaggle-ready CSV. That's the point.

| Problem | Fix |
|---|---|
| `Gender` was free text — 30+ spellings (`"M"`, `"male"`, `"Cis Female"`, `"woman"`...) | Standardized into `Male` / `Female` / `Other` |
| `Age` had garbage values (negatives, a 100-billion-year-old respondent) | Filtered to a realistic 15-80 range |
| `work_interfere` had missing values, but not randomly: it's blank because that person has no mental health condition to interfere with work | Filled as its own category, `"Not applicable"`, not dropped or imputed with mean/mode |
| `Country` had dozens of categories with just 1-2 responses each | Bucketed rare countries into `"Other"` |
| `comments`, `state`, `Timestamp` | Dropped — not predictive or too sparse |

## Pipeline

```
Raw survey.csv
      |
      v
Clean & handle missing data -> Encode categoricals -> Train/test split (80/20)
      |
      v
Train 4 models -> Compare (Accuracy, Precision, Recall, F1)
      |
      v
Feature importance (Random Forest) -> Visualize results
```

## How to Run

```bash
pip install pandas scikit-learn matplotlib
python model_comparison.py
```

Outputs:
- `results.csv` — full metrics table
- `model_comparison.png` — accuracy chart + top predictor chart

## Results

| Model | Accuracy | Precision | Recall | F1 Score |
|---|---|---|---|---|
| Random Forest | 84.9% | 82.5% | 89.0% | 85.6% |
| Logistic Regression | 82.1% | 83.1% | 81.1% | 82.1% |
| Decision Tree | 80.5% | 76.0% | 89.8% | 82.3% |
| KNN | 75.3% | 79.8% | 68.5% | 73.7% |

Random Forest wins — it handles the mix of categorical workplace factors
better than the simpler models, without overfitting like a single Decision
Tree.

## What Actually Predicts Treatment-Seeking?

Using Random Forest feature importance:

| Rank | Factor | Why it matters |
|---|---|---|
| 1 | `work_interfere` | By far the strongest signal — people whose condition disrupts their work are far more likely to seek help |
| 2 | Age | Older employees seek treatment more often |
| 3 | Family history | A known family history of mental illness increases likelihood significantly |
| 4 | Care options awareness | Knowing what your employer offers matters more than the size of the offering |
| 5 | Company size | Larger companies correlate with higher treatment rates |

Takeaway for employers: company size and generic benefits matter less than
you'd think. What moves the needle is whether employees recognize their
symptoms are affecting their work, and whether they know what support is
available.
