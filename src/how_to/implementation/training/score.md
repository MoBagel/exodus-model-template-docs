# Calculate a single set of scores

After we've trained a model, we want to be able to measure its performance. In our template this is done in the `calculate_score` function:

```python
def calculate_score(
    model: Union[LinearRegression, LogisticRegression], df: pd.DataFrame, target: Column, labels: np.ndarray
) -> Scores:
    """
    Calculates the scores for a frame and a model.

    The type of the model here should be modified.

    Parameters
    ----------
    model : Unknown
        The model to score with.
    df : pd.DataFrame
        The frame to score with.
    target : Column
        The target column.
    labels: np.ndarray
        The unique classes that were present in the training dataframe in a classification problem

    Returns
    -------
    Scores
        The scores for the model and the frame.
    """
    features = df.loc[:, df.columns != target.name]
    predicted = model.predict(X=features)
    actual = np.array(df[target.name].values)
    if target.data_type == DataType.double:
        # model type is LinearRegression here
        if not isinstance(model, LinearRegression):
            raise RuntimeError(
                f"Got {model.__class__} when it should be LinearRegression"
            )
        return RegressionScores.get_scores(predicted, actual)
    else:
        # model type is LogisticRegression here
        if not isinstance(model, LogisticRegression):
            raise RuntimeError(
                f"Got {model.__class__} when it should be LogisticRegression"
            )
        pred_proba = model.predict_proba(features)
        return ClassificationScores.get_scores(predicted, pred_proba, actual, labels)
```

Let's go through this step by step:
1. We extract the features from the given dataframe:
    ```python
        features = df.loc[:, df.columns != target.name]
    ```
2. We predict using the trained model and the extracted features:
    ```python
        predicted = model.predict(X=features)
    ```
3. We extract the actual values from the given dataframe:
    ```python
        actual = np.array(df[target.name].values)
    ```
4. If it is a regression problem, use the `RegressionScores` helper class to calculate the scores:
    ```python
            return RegressionScores.get_scores(predicted, actual)
    ```
5. Otherwise we use the `ClassificationScores` helper class to calculate:
    ```python
            pred_proba = model.predict_proba(features)
            return ClassificationScores.get_scores(predicted, pred_proba, actual, labels)
    ```

The `RegressionScores` and `ClassificationScores` helper classes will generate scores for you via the method `get_scores`, all you need to do is to provide the arguments.

## Implementing your own thing

Just like the `train_model` function, a lot of this is subject to which model you chose to use. Maybe your model does not have a `predict` method nor a `predict_proba` method, maybe your model needs more information than just the features to predict. Either way, it is up to you to decide what should be in this function. Once you have figured out how to predict using the trained model, the helper classes will automatically generate the scores.
