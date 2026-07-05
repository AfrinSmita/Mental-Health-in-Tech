# Mental Health in Tech — Predicting Who Seeks Treatment

A real-world (not toy) dataset project: predicting whether a tech-industry
employee has sought mental health treatment, based on workplace factors.

**Dataset:** OSMI Mental Health in Tech Survey (2014) — 1,259 responses
from tech employees worldwide. Originally from Kaggle
(`osmi/mental-health-in-tech-survey`).

## Why this dataset is a good portfolio choice
Unlike Titanic/Iris, this is a genuinely messy real survey:
- Free-text `Gender` field with dozens of inconsistent spellings
- `Age` column with garbage entries (negative numbers, typos like 100 billion)
- Structural missing values (`work_interfere` is blank *because* the person
  has no mental health condition — not random missingness)
- High-cardinality `Country` column

Handling these properly is the actual "data science" part of the project —
more valuable to show in an interview than fitting a model on clean data.

## What the script does
1. **Cleans the data**: drops unusable columns, filters invalid ages,
   standardizes messy gender text, fills structural missing values
   correctly, buckets rare countries into "Other"
2. **Encodes** categorical features
3. **Trains & compares 4 models**: Logistic Regression, KNN, Decision Tree,
   Random Forest
4. **Reports feature importance** — which factors actually predict
   treatment-seeking behavior


## Results

| Model | Accuracy | Precision | Recall | F1 Score |
|---|---|---|---|---|
| Random Forest | 84.9% | 82.5% | 89.0% | 85.6% |
| Logistic Regression | 82.1% | 83.1% | 81.1% | 82.1% |
| Decision Tree | 80.5% | 76.0% | 89.8% | 82.3% |
| KNN | 75.3% | 79.8% | 68.5% | 73.7% |

**Top predictors of seeking treatment** (from Random Forest):
1. `work_interfere` — whether their condition interferes with work (by far the strongest signal)
2. `Age`
3. `family_history` — family history of mental illness
4. `care_options` — whether they know what care options their employer offers
5. `no_employees` — company size

This makes intuitive sense: people who feel their mental health is actively
disrupting their work, or who have a family history, are far more likely to
seek treatment — while structural factors like company size and awareness
of care options matter, but less.

