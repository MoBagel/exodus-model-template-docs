# Enums

## `TimeUnit`

The time unit that Exodus accepts. Has multiple helper functions:
- `to_seasonality`: how many time units there is in a recurring pattern. For example, the seasonality of time unit `Month` would be `12`, since it's 12 months in a year.
- `to_resample_rule`: returns a string literal, which will then be used by pandas to resample the dataset.
- `to_time_delta_unit`: returns a string literal, which will then be used by pandas as time delta unit.
- `format_datetime`: formats a `datetime` to `str` according to this `TimeUnit`.
- `to_prediction_format`: returns the format required for prediction for this `TimeUnit`.

## `DataType`

The data type for a column in a dataframe. Could be either one of the following:
- `double`: the column is a numeric column.
- `timestamp`: the column is a datetime column.
- `string`: the column is a categorical column.
- `id`: the column is for IDs. Not really used.

It also comes with 2 helper methods:
- `to_type`: returns the Python type for this `DataType`. For instance, `DataType.double.to_type() == float`.
- `from_pandas`: turns the pandas type or array into a `DataType` instance. For example, `DataType.from_pandas(np.array([1,2,3,4,5])) == DataType.double`
