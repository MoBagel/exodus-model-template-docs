# Scores

There are 2 types of scores: `RegressionScores` and `ClassificationScores`. Instead of using them directly, you should use the class methods of these two classes:
- For `RegressionScores`, create it via `RegressionScores.get_scores`
- For `ClassificationScores`, create it via `ClassificationScores.get_scores`

## `RegressionScores`

Contains the following metrics:
- `mse` (mean square error)
- `rmse` (root mean square error)
- `rmsle` (root mean square logarithmic error)
- `mae` (mean absolute error)
- `r2` ([r squared](https://www.investopedia.com/terms/r/r-squared.asp))
- `deviance` ([deviance](https://en.wikipedia.org/wiki/Deviance_(statistics)), but here it's the same as `r2`)
- `mape` (mean absolute percentage error)
- `wmape` (weighted mean absolute percentage error)

## `ClassificationScores`

Could be either `BinomialScores` or `MultinomialScores`.

### `BinomialScores`

Contains the following metrics:
- `logloss`
- `mean_per_class_error` (mean per class error)
- `misclassification`
- `auc` (area under curve)
- `lift_top_group`

### `MultinomialScores`

Contains the following metrics:
- `logloss`
- `mean_per_class_error` (mean per class error)
