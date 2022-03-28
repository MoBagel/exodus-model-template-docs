# Feature engineering

After we've ensured that there's nothing unprocessable in our input, we can move on to the feature engineering step, where we manipulate the input data to generate more suitable training input.

```python
        # then, do feature engineering
        # TODO decide what feature engineering steps you need by modifying `feature_engineering` method
        training_df, holdout_df, encoders, components = feature_engineering(
            sanitized_df, sanitized_holdout
        )
```

If you right click on the `feature_engineering` symbol and select `Go to definition`, you should be able to see that this is a method defined in `model_algorithm.py` as well.

```python
def feature_engineering(training_df: pd.DataFrame, holdout_df: Optional[pd.DataFrame]):
    """
    Does feature engineering for the dataframes.

    Here you should use the helper methods in `exodusutils` package, i.e. `time_component_encoding`, `one_hot_encoding` and `label_encoding`.

    Parameters
    ----------
    training_df : pd.DataFrame
        The training dataframe
    holdout_df : Optional[pd.DataFrame]
        The holdout dataframe

    Returns
    -------
    tuple[DataFrame, Optional[DataFrame], Dict[str, LabelEncoder], List[str]]
        The modified training df and holdout df, the encoders for each categorical column, and the time component columns.
    """
    training_df, holdout_df, encoders = label_encoding(training_df, holdout_df)
    training_df, components = time_component_encoding(training_df)
    # datetime columns are being removed because the necessary features derived from them
    # have been generated in the previous step
    # However, it is up to the developer to decide whether or not this step is necessary
    training_df = remove_datetime_columns(training_df)
    if holdout_df is not None:
        holdout_df, _ = time_component_encoding(holdout_df)
        holdout_df = remove_datetime_columns(holdout_df)
    return training_df, holdout_df, encoders, components
```

The exact procedures for feature engineering is entirely up to you, and the code shown here is really just an example. Let's break it down step by step.
