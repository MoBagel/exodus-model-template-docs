# Constants

## `DATETIME_COMPONENTS_PREFIX `

The prefixes for datetime component columns. For example, if a column named `foo` is a datetime column, then after `time_component_encoding`, we get the following new columns:
- `YEAR_foo`
- `QUARTER_foo`
- `MONTH_foo`
- `WEEK_foo`
- `WEEKDAY_foo`
- `DAY_foo`
- `HOUR_foo`

## `PREDICTION_COLNAME`

The name of the prediction column. Essentially the string literal `"prediction"`.
