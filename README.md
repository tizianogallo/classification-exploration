# 🔍 Classification Exploration: Predicting Employee Attrition

**Author:** Tiziano Gallo

A hands-on comparison of seven classification algorithms, each in its own Jupyter notebook, applied to the same problem: predicting which employees are likely to leave a company. Every notebook follows the same workflow, from data preparation and a first model to hyperparameter tuning, evaluation and interpretation, so the models can be compared like for like.

---

## 📌 Overview

The goal is to learn how different classifiers behave on a realistic, imbalanced business problem, not just to maximise a score. Each notebook trains a model, tunes it with cross-validated grid search, checks it for overfitting, and ends with a short written summary and an updated model leaderboard.

---

## 🗂️ Dataset

**IBM HR Analytics Employee Attrition & Performance** ([Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)), a fictional dataset created by IBM data scientists.

| | |
|---|---|
| **Records** | 1,470 employees |
| **Features** | 35 columns covering demographics, job role, satisfaction levels and compensation |
| **Target** | `Attrition` (Yes / No) |
| **Class balance** | 237 left (16.1%) vs 1,233 stayed (83.9%) |
| **Missing values** | None |

The file is in [`data/`](data/WA_Fn-UseC_-HR-Employee-Attrition.csv).

---

## ⚙️ Workflow

1. **Cleaning:** drop `EmployeeCount`, `EmployeeNumber`, `StandardHours` and `Over18`, which carry no information.
2. **Encoding:** one-hot encode the 7 categorical columns (`drop_first=True`), which gives 44 features. Encode the target as Yes = 1, No = 0.
3. **Split:** 70/30 stratified train/test split (1,029 / 441 rows, `random_state=123`) so the class imbalance is preserved in both sets.
4. **Baseline model** with default or hand-picked parameters.
5. **Tuning:** `GridSearchCV` with cross-validation, scored on F1 for the minority class (recall for Logistic Regression).
6. **Evaluation:** F1, precision, recall, confusion matrix, ROC curve and AUC on the test set, compared against the training score to spot overfitting.
7. **Interpretation and visualisation** where the model allows it, such as tree plots, coefficients, feature importances, and PCA or t-SNE decision maps.

Because 84% of employees stay, a model that always predicts "No" is already 84% accurate. In the KNN notebook, the untuned k = 40 model does exactly that and scores F1 = 0. For this reason the notebooks focus on **F1 and AUC**, not accuracy.

---

## 📓 Notebooks and Results

| # | Notebook | Model | Best configuration | Train F1 | Test F1 | AUC |
|---|---|---|---|---|---|---|
| 01 | [`KNN`](KNN/01_CE_KNN.ipynb) | K-Nearest Neighbors | k = 1, standardised features | 1.00 | 0.22 | 0.54 |
| 02 | [`DecisionTree`](DecisionTree/02_CE_DecisionTree.ipynb) | Decision Tree | entropy, max_depth = 50, min_samples_split = 10, min_samples_leaf = 2 | 0.85 | 0.44 | 0.69 |
| 03 | [`Naive_Bayes`](Naive_Bayes/03_CE_Naive_Bayes.ipynb) | Gaussian Naive Bayes | default | 0.44 | 0.44 | 0.74 |
| 04 | [`Logistic_Regression`](Logistic_Regression/04_CE_LogisticRegression.ipynb) | Logistic Regression | C = 1, L2 penalty | 0.57 | **0.61** | **0.86** |
| 05 | [`SVM`](SVM/05_CE_SVM.ipynb) | Support Vector Machine | linear kernel, C = 1 | 0.55 | 0.60 | **0.86** |
| 06 | [`ML_Perceptron`](ML_Perceptron/06_CE_ML_Perceptron.ipynb) | Multi-Layer Perceptron | hidden layers (5, 3, 2), alpha = 0.1 | 0.84 | 0.59 | 0.77 |
| 07 | [`Ensemble`](Ensemble/07_CE_Ensemble.ipynb) | Random Forest | 100 trees, entropy, unrestricted depth | 0.98 | 0.31 | 0.82 |
| 07 | [`Ensemble`](Ensemble/07_CE_Ensemble.ipynb) | AdaBoost | 200 estimators, learning_rate = 1 | 0.58 | 0.55 | 0.83 |
| 07 | [`Ensemble`](Ensemble/07_CE_Ensemble.ipynb) | Gradient Boosting | 50 estimators, learning_rate = 1 | 1.00 | 0.48 | n/a |

Each notebook folder also contains the plots it produced, such as ROC curves, confusion matrices and feature plots.

---

## 🔑 Key Findings

- **Simple linear models won.** Logistic Regression (F1 0.61, AUC 0.86) and a linear-kernel SVM (F1 0.60, AUC 0.86) were the strongest, which suggests the signal in this data is largely linear.
- **More complex did not mean better.** The MLP, Random Forest and Gradient Boosting all overfit heavily, with training F1 of 0.84 or higher against much lower test F1 (the same happened with KNN and the Decision Tree). On a dataset of only 1,470 rows, the simpler models generalised better.
- **Naive Bayes was the surprise.** With no tuning at all, it showed no overfitting (train and test F1 both 0.44) and an AUC of 0.74, ahead of the Decision Tree and KNN.
- **Class imbalance is the core challenge.** Even the best models catch only about half of the employees who leave (recall around 0.49 for Logistic Regression and SVM).
- **What predicts attrition.** In the Logistic Regression coefficients, working overtime is the strongest signal, followed by frequent business travel and being single. Higher job involvement and higher environment and job satisfaction point toward staying. The Random Forest ranks monthly income, age, total working years and daily rate highest.

---

## ⚠️ Limitations and Next Steps

- A single train/test split, so results can move with a different random seed.
- No explicit handling of class imbalance. Class weights or SMOTE are natural next steps, as is tuning the decision threshold for recall.
- No AUC was computed for Gradient Boosting.
- Possible extensions include XGBoost, stacking, and non-tree weak learners for bagging and boosting.

---

## 📁 Repository Structure

```
classification-exploration/
│
├── data/
│   └── WA_Fn-UseC_-HR-Employee-Attrition.csv
├── KNN/                  # 01_CE_KNN.ipynb + ROC and train/test curves
├── DecisionTree/         # 02_CE_DecisionTree.ipynb + ROC and max_depth curve
├── Naive_Bayes/          # 03_CE_Naive_Bayes.ipynb + ROC and PCA plot
├── Logistic_Regression/  # 04_CE_LogisticRegression.ipynb + ROC, coefficients, PCA plot
├── SVM/                  # 05_CE_SVM.ipynb + ROC and t-SNE plot
├── ML_Perceptron/        # 06_CE_ML_Perceptron.ipynb + ROC and confusion matrix
└── Ensemble/             # 07_CE_Ensemble.ipynb + AdaBoost ROC and RF feature importances
```

---

## 🛠️ Tools & Technologies

![Python](https://img.shields.io/badge/Python-Language-3776AB) ![pandas](https://img.shields.io/badge/pandas-Data-150458) ![NumPy](https://img.shields.io/badge/NumPy-Numerics-013243) ![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E) ![matplotlib](https://img.shields.io/badge/matplotlib-Plotting-11557C) ![seaborn](https://img.shields.io/badge/seaborn-Plotting-4C72B0) ![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626)
