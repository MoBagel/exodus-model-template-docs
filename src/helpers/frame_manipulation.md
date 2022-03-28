# Frame manipulation

Contains methods that are applying feature engineering artifacts to the prediction dataframe.
- `apply_label_encoders`
- `apply_one_hot_encoding`

Also contains these helper methods:
- `to_numeric_features_df`: extracts the numeric features from the dataframe, and returns them as a new dataframe.
- `remove_invalid_values`: removes the invalid values from the dataframe.
- `remove_datetime_columns`: removes all columns of dtype `datetime64`.
- `fill_nan_with_mode`: fills the `NaN` cells in each numeric column with the most frequent value (aka `mode`) of that column.
