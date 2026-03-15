# 01 - K-Nearest Neighbors (KNN)

## What is KNN?
KNN is one of the simplest classification algorithms out there. It works by looking at the k closest data points to a new observation and assigning it the most common class among those neighbors. No training is needed — the model just memorizes the data and makes decisions at prediction time based on distance.

The main hyperparameter is **k** — too small and the model overfits, too large and it underfits.

## When to use KNN
- Small, low-dimensional datasets
- When you need a simple baseline model
- When the decision boundary is irregular and non-linear
- When interpretability is not a priority

## Limitations
- Slow at prediction time — has to compute distances to all training points
- Sensitive to irrelevant features and different scales — always scale your data
- Struggles with high-dimensional data (curse of dimensionality)
- No built-in way to handle class imbalance
- Not probabilistic — harder to calibrate confidence

## Results

| Metric | Train | Test |
|--------|-------|------|
| F1 Score | 1.00 | 0.22 |
| AUC | - | 0.54 |

## What we found
KNN really struggled with this dataset. Without scaling it predicted only the majority class (No attrition), giving F1=0. After adding StandardScaler and tuning k with GridSearchCV, the best result was k=1 with F1=0.22 and AUC=0.54 — barely better than random guessing.

The core issues are:
- **Class imbalance** (84% No vs 16% Yes) — KNN has no built-in way to handle this
- **High dimensionality** — after encoding, distance metrics lose effectiveness
- **k=1 won** — meaning the model is essentially memorizing the nearest point rather than generalizing

## Plots

### Train/Test F1 Curve
![KNN Train Test Curve](../outputs/01_knn_train_test_curve.png)

The test F1 peaks at very small k values and flatlines to 0 after k=40 — there is no sweet spot where the model generalizes well.

### ROC Curve
![KNN ROC Curve](../outputs/01_knn_roc.png)

AUC of 0.54 confirms the model has almost no discriminative power on this dataset.
