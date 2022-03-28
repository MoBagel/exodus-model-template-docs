# Miscellaneous

- `closest_string`: given a list of strings and one reference string, find out which string is the closest to the reference in the list.
- `remove_invalid_targets`: returns a new dataframe that does not have `NaN` in target column. Does not modify the original dataframe.
- `format_datetime_column_with_unit`: turns date column into `str` based on given `time_unit`. Will modify the original `df`.
- `are_valid_values`: whether there exists a non-`np.nan` value in `values`. Used to test if a fold is useless - if all actual values are `np.nan`, then the fold is not usable.
