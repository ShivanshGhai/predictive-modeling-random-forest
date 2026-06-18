# STAT 452 Statistical Learning and Prediction Final Project

This project compares several regression and machine learning methods for predicting a continuous response variable from 19 numeric predictors. The final selected model is a tuned random forest, chosen after comparing linear models, shrinkage methods, dimension-reduction methods, trees, random forests, and gradient boosting.

The original assignment required a reproducible Quarto/R Markdown workflow, a short report explaining model selection, and a one-column prediction CSV. This repository keeps the reproducible source and public-facing report while excluding the course-provided data and generated prediction files.

## Results Summary

The tuned random forest was the strongest model overall, with an out-of-bag error of approximately `15.84 MSE`, equivalent to about `3.98 RMSE`. The next-best tree-based alternatives were weaker, including the caret random forest at about `4.01 RMSE` and GBM at about `4.44 RMSE`.

The main result is available in the Markdown report, which previews directly on GitHub:

- [Project Report](REPORT.md)

A sanitized PDF version is also included as a downloadable artifact:

- [Project Report PDF](output/pdf/project-report.pdf)

## Project Contents

- `finalcode.qmd`: Full reproducible Quarto analysis with model fitting, tuning, comparison, and final prediction generation.
- `projectcode.R`: R script version of the modeling workflow.
- `report.qmd`: Written project report summarizing the modeling approach and results.
- `REPORT.md`: GitHub-readable version of the final report.
- `requirements.R`: Helper script for installing required R packages.
- `output/pdf/project-report.pdf`: Sanitized report PDF retained as an optional download.

## Assignment Requirements Covered

- Models tried: OLS, ridge, LASSO, elastic net, PCR, PLS, regression trees, random forest, and GBM.
- Evaluation process: 10-fold cross-validation for most models and OOB MSE for the manually tuned random forest.
- Tuning: grids for shrinkage, random forest, regression tree pruning, PCR/PLS components, and GBM settings.
- Predictor importance: LASSO coefficient shrinkage and random forest importance.
- Prediction output: final code writes `my_predictions.csv` as one column with no row names and no header.

## Methods Used

- Ordinary least squares
- Ridge regression
- LASSO regression
- Elastic net regression
- Principal components regression
- Partial least squares regression
- Regression trees
- Random forest
- Gradient boosting machines

## Final Model

The best-performing model was a manually tuned random forest using:

- `mtry = 11`
- `nodesize = 2`
- `ntree = 1500`

The final model is trained on the full training set and used to generate predictions for the provided test predictors.

## Data

The original training and test CSV files are not included in this repository by default because they appear to be course-provided data. To reproduce the analysis, place the following files in the project root:

- `training_data.csv`
- `test_predictors.csv`

The expected training data contains a response column named `Y` and predictor columns named `X1` through `X19`. The test data contains the predictor columns only.

## R Packages

The analysis uses the following R packages:

- `caret`
- `glmnet`
- `randomForest`
- `gbm`
- `pls`
- `gam`
- `rpart`
- `rpart.plot`
- `splines`

## Reproducing

Install required packages:

```r
source("requirements.R")
```

Render the main Quarto file:

```r
quarto::quarto_render("finalcode.qmd")
```

Or run the R script directly:

```r
source("projectcode.R")
```

The final prediction file is written as `my_predictions.csv`.
