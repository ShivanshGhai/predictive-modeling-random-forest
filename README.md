# STAT 452/652 Predictive Modeling Project

This project compares several regression and machine learning methods for predicting a continuous response variable from 19 numeric predictors. The final selected model is a tuned random forest, chosen after comparing linear models, shrinkage methods, dimension-reduction methods, trees, random forests, and gradient boosting.

## Project Contents

- `finalcode.qmd`: Full reproducible Quarto analysis with model fitting, tuning, comparison, and final prediction generation.
- `projectcode.R`: R script version of the modeling workflow.
- `report.qmd`: Written project report summarizing the modeling approach and results.

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

Render the main Quarto file:

```r
quarto::quarto_render("finalcode.qmd")
```

Or run the R script directly:

```r
source("projectcode.R")
```

The final prediction file is written as `my_predictions.csv`.

