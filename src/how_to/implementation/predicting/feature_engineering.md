# Apply feature engineering

```python
def apply_feature_engineering(df: pd.DataFrame, encoders: Dict[str, LabelEncoder]) -> pd.DataFrame:
    encoded: pd.DataFrame = (df.pipe(apply_label_encoders, encoders).pipe(time_component_encoding))[0]
    return encoded.pipe(fill_nan_with_average, list(encoders.keys())).pipe(remove_datetime_columns)

# ... snipped ...

        df = apply_feature_engineering(sanitized_df, model_info.encoders)
```

Most of the things you are doing here are exactly the same as the `feature_engineering` part, except here you need to make sure you sanitize your dataframe properly.

## Sanitize the result properly

Consider the following training dataframe, where there is only 1 column:

|column|
|------|
| foo |
| bar |
| baz |

Then after the column has been label encoded, the result becomes:

|column|
|------|
| 0 |
| 1 |
| 2 |

Where `foo` gets encoded to `0`, `bar` becomes `1`, and `baz` becomes `2`.

However, if the prediction dataframe contains a value never seen during training, the encoder will not be able to deduce which label it should encode the value to, and will return a `NaN`. For example, consider the below dataframe:

|column|
|------|
| bar |
| quax |
| bar |
| foo |

After we apply our encoder, the result is:

|column|
| -----|
| 1 |
| nan |
| 1 |
| 0 |

If your machine learning algorithm cannot handle `NaN` properly, then after you've applied the feature engineering encoders there is no way for the algorithm to perform prediction.

In situations like this, a common method is to impute the missing values with a designated special value. In `exodusutils` the method `fill_nan_with_mode` is doing just that: we extract the most frequent label for an encoded column, and force the invalid cells to that most frequent label.

In our example, the final result after we've applied the `fill_nan_with_mode` method will be:

|column|
| -----|
| 1 |
| 1 |
| 1 |
| 0 |
