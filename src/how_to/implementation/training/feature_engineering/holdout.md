# Handle holdout dataframe

If the holdout dataframe exists, we perform the same feature engineering on it as the training dataframe.
```python
    if holdout_df is not None:
        holdout_df, _ = time_component_encoding(holdout_df)
        holdout_df = remove_datetime_columns(holdout_df)
```
