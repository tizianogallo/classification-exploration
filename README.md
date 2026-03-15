# Classification Exploration

A hands-on exploration of 7 classic classification algorithms applied to the IBM HR Analytics Employee Attrition dataset. Each notebook follows the same structure — data prep, model training, hyperparameter tuning with GridSearchCV, evaluation, and visualization — making it easy to compare approaches side by side.

---

## Dataset
The [IBM HR Analytics Employee Attrition dataset](https://www.kaggle.com/pavansubhasht/ibm-hr-analytics-attrition-dataset) contains 1,470 employee records with 35 features covering demographics, job roles, satisfaction levels, and compensation. The target variable is **Attrition** — whether an employee left the company (Yes/No).

The dataset is imbalanced — **84% No vs 16% Yes** — which makes this a challenging and realistic classification problem.

---

## Notebooks

### 01 - K-Nearest Neighbors
- Small datasets with low dimensionality
- When you need a simple baseline
- When the decision boundary is irregular and non-linear

[01_knn.ipynb](notebooks/01_knn.ipynb) • [README](notebooks/01_knn_README.md)

---

### 02 - Decision Tree
- When interpretability is important
- When you have mixed data types
- When you don't want to scale your data

[02_decision_tree.ipynb](notebooks/02_decision_tree.ipynb) • [README](notebooks/02_decision_tree_README.md)

---

### 03 - Naive Bayes
- When training data is limited
- When you need fast, lightweight predictions
- When features are roughly independent

[03_naive_bayes.ipynb](notebooks/03_naive_bayes.ipynb) • [README](notebooks/03_naive_bayes_README.md)

---

### 04 - Logistic Regression
- When you need a strong, interpretable baseline
- When the decision boundary is roughly linear
- When feature importance via coefficients matters

[04_logistic_regression.ipynb](notebooks/04_logistic_regression.ipynb) • [README](notebooks/04_logistic_regression_README.md)

---

### 05 - Support Vector Machine
- When the number of features is high relative to samples
- When there is a clear margin of separation between classes
- When you need a robust model with scaling

[05_svm.ipynb](notebooks/05_svm.ipynb) • [README](notebooks/05_svm_README.md)

---

### 06 - Multi-Layer Perceptron
- When you have large amounts of data
- When the relationship between features and target is highly non-linear
- When you have compute resources to tune the architecture

[06_mlp.ipynb](notebooks/06_mlp.ipynb) • [README](notebooks/06_mlp_README.md)

---

### 07 - Ensemble Methods
- When a single model consistently overfits or underfits
- When you need to squeeze out extra performance on tabular data
- When you have enough compute for longer training times

[07_ensemble.ipynb](notebooks/07_ensemble.ipynb) • [README](notebooks/07_ensemble_README.md)

---

## Final Leaderboard

| Model | F1 Test | AUC |
|-------|---------|-----|
| KNN | 0.22 | 0.54 |
| Decision Tree | 0.44 | 0.69 |
| Naive Bayes | 0.44 | 0.74 |
| MLP | 0.59 | 0.77 |
| Random Forest | 0.31 | 0.82 |
| AdaBoost | 0.55 | 0.83 |
| Gradient Boosting | 0.48 | N/A |
| SVM | 0.60 | 0.86 |
| **Logistic Regression** | **0.61** | **0.86** |

---

## Repo Structure
```
classification-exploration/
├── data/
│   └── WA_Fn-UseC_-HR-Employee-Attrition.csv
├── notebooks/
│   ├── 01_knn.ipynb
│   ├── 01_knn_README.md
│   ├── 02_decision_tree.ipynb
│   ├── 02_decision_tree_README.md
│   ├── 03_naive_bayes.ipynb
│   ├── 03_naive_bayes_README.md
│   ├── 04_logistic_regression.ipynb
│   ├── 04_logistic_regression_README.md
│   ├── 05_svm.ipynb
│   ├── 05_svm_README.md
│   ├── 06_mlp.ipynb
│   ├── 06_mlp_README.md
│   ├── 07_ensemble.ipynb
│   └── 07_ensemble_README.md
├── outputs/
│   ├── 01_knn_train_test_curve.png
│   ├── 01_knn_roc.png
│   └── ...
└── README.md
```

---

## Key Findings

- **Logistic Regression won** — the best F1 (0.61) and AUC (0.86) with zero overfitting. The data is largely linearly separable, which explains why linear models dominated throughout.
- **OverTime is the strongest predictor** — employees working overtime are by far the most likely to leave, followed by frequent business travel and being single.
- **Ensemble methods disappointed** — Random Forest and Gradient Boosting both severely overfit. Only AdaBoost was competitive, suggesting that boosting handles class imbalance better than bagging on this dataset.
- **Complexity didn't help** — MLP and ensemble methods underperformed compared to simple linear models. With only 1,470 samples, simpler models generalize better.
- **Class imbalance is the core challenge** — 84/16 split means accuracy is a misleading metric. F1 score and AUC were used throughout to get a fairer picture.

---

## Future Work

- **Handle class imbalance explicitly** — techniques like SMOTE, class weighting (`class_weight='balanced'`), or threshold tuning could significantly improve all models
- **Feature engineering** — creating interaction features like OverTime × BusinessTravel or satisfaction composite scores
- **XGBoost / LightGBM** — more powerful boosting implementations that might outperform the sklearn ensemble methods
- **Cross-validation strategy** — using stratified k-fold throughout to get more reliable estimates
- **Calibration** — probability calibration for models like SVM to improve confidence estimates

---

## Requirements
```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```
