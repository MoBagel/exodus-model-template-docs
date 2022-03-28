# Sanitizing input

```python
        original_df = request.get_prediction_df(model_info.feature_types)
        sanitized_df = remove_invalid_values(original_df)
```

This should be self-explanatory. Basically, here your code should be doing what you did to the training dataframe.

Notice how we have access to the prediction dataframe via `get_prediction_df` method from the `PredictReqBody` class.
