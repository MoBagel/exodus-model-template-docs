# Label encoding

```python
    training_df, holdout_df, encoders = label_encoding(training_df, holdout_df)
```

Label encoding is the procedure that turns a categorical column into a numeric one, with the numbers being the label for a categorical value. Here we have to make sure the generated columns are consistent between the training dataframe and the holdout dataframe, hence we pass both into the method. It handles the case when the holdout dataframe is actually `None`.
