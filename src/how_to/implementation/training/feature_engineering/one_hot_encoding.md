# One-hot encoding

One-hot encoding is another encoding technique to transform categorical columns. More precisely, it encodes a categorical column into multiple numeric columns, each representing one value in the categorical column.

This encoding can be invoke by doing:
```python
    training_df, holdout_df, encoded_columns = one_hot_encoding(training_df, holdout_df)
```

You should choose either one of one-hot encoding and label encoding if you want to transform categorical columns.
