# STAT 452/652 Final Project Report

**Author:** Shivansh Ghai  
**Date:** December 4, 2025

## 1. Introduction

The purpose of this project is to build an accurate predictive model for a continuous response **Y** using a training dataset of 20,000 observations and approximately 19 numeric predictors (`X1`-`X19`). A separate test dataset containing only predictor values was provided. The goal was to generate predictions that minimize the **Mean Squared Prediction Error (MSPE)**.

Following the STAT 452/652 syllabus, I explored a range of linear, shrinkage, dimension-reduction, and ensemble methods. After model comparison and hyperparameter tuning, a **random forest with `mtry = 11` and `nodesize = 2`** achieved the strongest estimated out-of-sample predictive performance and was selected as the final model.

## 2. Models Evaluated

The project compared methods from the course scope, including linear models, shrinkage methods, dimension-reduction approaches, and tree-based ensembles.

### Linear and Shrinkage Models

- Ordinary least squares (OLS)
- Ridge regression
- LASSO regression
- Elastic net regression

### Dimension-Reduction Approaches

- Principal components regression (PCR)
- Partial least squares regression (PLS)

### Tree-Based and Ensemble Methods

- Regression trees (`rpart`)
- Random forest using `caret`
- Manually tuned random forest using OOB error
- Gradient boosting machines using `gbm`
- Gradient boosting machines using `caret`

## 3. Model Evaluation

Linear, shrinkage, dimension-reduction, and boosting methods were evaluated with **10-fold cross-validation**. For the manually tuned random forest, **out-of-bag (OOB) MSE** was used as the internal validation estimate.

| Model | CV RMSE / MSE | Notes |
|---|---:|---|
| OLS | ~5.19 RMSE | Baseline |
| Ridge | ~5.22 RMSE | Slightly worse |
| LASSO | ~5.19 RMSE | Comparable to OLS |
| Elastic Net | ~26 MSE | Not competitive |
| PCR | ~5.19 RMSE | No improvement |
| PLS | ~5.19 RMSE | Similar |
| Regression Tree | ~26 MSE | Underfit |
| Random Forest (`caret`) | **4.01 RMSE** | Strong performer |
| Tuned Random Forest (OOB) | **15.84 MSE (~3.98 RMSE)** | **Best overall** |
| GBM (`caret`) | 4.44 RMSE | Good but weaker |

The manually tuned random forest outperformed the other candidate models and was therefore chosen as the final predictive model.

## 4. Parameter Tuning

### Shrinkage Models

Ridge, LASSO, and elastic net were tuned over grids of `lambda` values. Elastic net also tuned `alpha`. The best tuning values were selected based on cross-validated error.

### PCR and PLS

PCR and PLS used cross-validation to determine the number of components through validation error curves.

### Regression Trees

The regression tree was pruned using the 1-SE rule, selecting a simpler subtree within one standard error of the minimum cross-validated error.

### Random Forest

The `caret` random forest evaluated:

```r
mtry = c(3, 5, 7, 9, 11)
```

The best cross-validated RMSE occurred at `mtry = 11`.

The manual random forest grid then evaluated combinations of `mtry` and `nodesize` using OOB MSE. The best settings were:

- `mtry = 11`
- `nodesize = 2`

The final model used these values with `ntree = 1500`.

### Gradient Boosting

GBM tuning considered interaction depth, shrinkage, number of trees, and minimum observations per node. Although GBM performed reasonably well, its best result was weaker than the tuned random forest.

## 5. Important Predictors

Two primary tools informed the assessment of influential predictors:

- LASSO coefficient shrinkage
- Random forest variable importance

LASSO retained 17 predictors, though several coefficients were small. Random forest importance scores suggested a smaller core group of important predictors:

```text
X11, X13, X1, X8, X12, X5, X15, X9, X19
```

Combining both sources, a reasonable estimate is that **8-12 predictors** are meaningfully influential in generating `Y`.

## 6. Final Model

The final model was fit on the full training dataset:

```r
set.seed(452)
library(randomForest)

train <- read.csv("training_data.csv")
test  <- read.csv("test_predictors.csv")

X <- train[, grep("^X", names(train))]
y <- train$Y

best_mtry <- 11
best_nodesize <- 2

final_rf <- randomForest(
  x = X,
  y = y,
  mtry = best_mtry,
  ntree = 1500,
  nodesize = best_nodesize,
  importance = TRUE
)

final_predictions <- predict(final_rf, newdata = test)

write.table(
  final_predictions,
  "my_predictions.csv",
  sep = ",",
  row.names = FALSE,
  col.names = FALSE
)
```

## 7. Conclusion

Across the evaluated models, the tuned **random forest** achieved the strongest predictive accuracy, outperforming linear, shrinkage, dimension-reduction, and boosting approaches. Its low OOB MSE and stable validation behavior made it the best choice for final predictions.

