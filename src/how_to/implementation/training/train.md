# Train the machine learning model

Let's take a look at the `train_model` function, where we actually train a machine learning model:

```python
def train_model(train_frames: TrainFrames, target: Column):
    """
    Trains a model.

    Parameters
    ----------
    train_frames : TrainFrames
        The frames required during training. Includes the training dataframe, the validation dataframe, and the testing dataframe.
    target : Column
        The target column, tells you the name and the type of the column.

    Returns
    -------
    model
        The trained model.
    """
    train = train_frames.train
    features = train.loc[:, train.columns != target.name]
    targets = train[target.name]
    if target.data_type == DataType.double:
        model = LinearRegression()
    else:
        model = LogisticRegression()
    model.fit(X=features, y=targets)
    return model
```

Here, we extract the sub-frame that we will train the model with:
```python
    train = train_frames.train
```

And then split the sub-frame into one dataframe with features to train with, and another one with only the target values to fit the machine learning model:
```python
    features = train.loc[:, train.columns != target.name]
    targets = train[target.name]
```

If we are dealing with a regression problem, where the target column consists of numeric values, we use `LinearRegression` algorithm. Otherwise we are dealing with a classification problem, and we use `LogisticRegression` algorithm.
```python
    if target.data_type == DataType.double:
        model = LinearRegression()
    else:
        model = LogisticRegression()
```

After we decide which algorithm to use, we can fit the model, and then return it:
```python
    model.fit(X=features, y=targets)
    return model
```

## Implementing your own thing

Most of the code here should be changed by you. In particular these are the things you need to consider:
- Perhaps your machine learning algorithm is able to handle both classification and regression problems
- Your model needs to filter out some more features
- Instead of `fit`, your model needs to do something else
- Your model requires a validation dataframe, which can be retreived via `train_frames.validation`

Either way, the content of this function should only serve as an example, it is up to you to decide what should actually be the machine learning model.
