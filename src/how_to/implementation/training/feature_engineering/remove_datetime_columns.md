# Remove datetime columns

Once we have done time component encoding, for our algorithm the datetime column is no longer needed.

```python
    training_df = remove_datetime_columns(training_df)
```
