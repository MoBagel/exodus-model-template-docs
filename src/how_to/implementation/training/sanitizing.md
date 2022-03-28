# Sanitizing input

It could be the case that the input data contains unprocessable characters for the machine learning algorithm. For example, the model algorithm template's algorithm cannot process datasets with `NaN` or empty strings. To fix that, we start out the training process by sanitizing the dataset:
```python
        # TODO: The implementer of algorithms should decide if this step is necessary, or if an alternative method should be used instead
        # First sanitize the dataframe
        sanitized_df = remove_invalid_values(request.get_training_df())
        raw_holdout_df = request.get_holdout_data()
        if raw_holdout_df is not None:
            sanitized_holdout = remove_invalid_values(raw_holdout_df)
        else:
            sanitized_holdout = None
```

Notice how we are not parsing the request here, instead there already exists helper functions `get_training_df` and `get_holdout_data`.

Since our model algorithm cannot handle invalid values, we can invoke the `remove_invalid_values` function from the `exodusutils` package to do that for us.

If the machine learning algorithm you chose has different restrictions, you should implement them here.

Note that `holdout_data` is actually an optional field for training, and we do not want to sanitize a nonexist dataset. So you want to make sure you sanitize it only when you're sure it is not a `None`.
