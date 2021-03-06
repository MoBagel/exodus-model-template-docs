# Format the results

After we have our raw prediction results, we need to piece it back into a JSON string.

```python
        if model_info.target.data_type != DataType.double:
            predicted = model_info.encoders[model_info.target.name].inverse_transform(predicted)

        sanitized_df[PREDICTION_COLNAME] = predicted

        if model_info.target.data_type != DataType.double:
            classes = list(model_info.model.classes_)
            predict_proba = pd.DataFrame(model_info.model.predict_proba(features), columns=classes)
            sanitized_df = pd.concat([sanitized_df, predict_proba], axis=1).reset_index(drop=True)
        else:
            classes = list()

        columns_to_drop = [
            c
            for c in list(sanitized_df.columns)
            if c not in request.keep_columns and c != PREDICTION_COLNAME and c not in classes
        ]

        results: List[Dict[str, Any]] = json.loads(
            str(sanitized_df.drop(columns=columns_to_drop).to_json(orient="records"))
        )
```

The return value of the `predict` method contains only a field of type `List[Dict[str, Any]]`, where a single `Dict` represents the prediction results for a single row in the prediction input dataframe. That being said, it is easier for us to manipulate on a dataframe than doing each `Dict` by hand, so let's see how we can achieve that.

## Turn our predicted value back to strings

Remember that we did `label_encoding` during feature engineering? This will turn our target column into a column with numeric class labels, and we need to turn those back into actual strings.

To do that, we make use of the `LabelEncoder.inverse_transform` method:

```python
        if model_info.target.data_type != DataType.double:
            predicted = model_info.encoders[model_info.target.name].inverse_transform(predicted)
```

If your model didn't do `label_encoding` during feature engineering, you can omit this step.

## Attach the predicted values

Then we attach our predicted value to the input dataframe.
```python
        sanitized_df[PREDICTION_COLNAME] = predicted
```

If we are dealing with a classification problem, each prediction should come with the prediction probabilities of all the possible classes. To do that, we create a new dataframe containing the prediction probabilities, and then concatenate the new dataframe to our result dataframe.

Note that we have to use the strings instead of the model's classes, which are just a bunch of numbers.
```python
        if model_info.target.data_type != DataType.double:
            classes = list(model_info.encoders[model_info.target.name].classes_)
            predict_proba = pd.DataFrame(model_info.model.predict_proba(features), columns=classes)
            sanitized_df = pd.concat([sanitized_df, predict_proba], axis=1).reset_index(drop=True)
        else:
            classes = list()
```

We also need to keep track of the classes we've added to the result dataframe.

## Drop columns we don't need

Then we need to filter out the columns we don't need for the result.
```python
        columns_to_drop = [
            c
            for c in list(sanitized_df.columns)
            if c not in request.keep_columns and c != PREDICTION_COLNAME and c not in classes
        ]
```

We only keep the columns that are explicitly specified by the user (via the `keep_columns` field in the request), the prediction result column (the `PREDICTION_COLNAME` variable, which is an alias for the string `"prediction"`), and the classes for the target column if it's a classification problem.
