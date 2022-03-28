# Appendix B: How cross validation scores are calculated

To create the frames for cross validation, `exodusutils` library provides a helper class called `CVFrames`:
```python
frames: List[TrainFrame] = CVFrames(df, nfolds, validation_percentage, shuffle=False).frames
```

Here, `df` is the training dataframe, `nfolds` is the number of folds you want for the cross validation, `validation_percentage` denotes the portion of training dataframe you want to be as the validation frame, and `shuffle` means whether the training dataframe will be shuffled before cutting.

In the return type, a `TrainFrame` is essentially a 3-tuple comprised of three pandas `DataFrame`s. The first is the dataframe that will be used to train the cross validation model, the second used as the validation set during training, the third as the testing set for calculating the cross validation score.

Note that a cross validation fold is invalid if it does not have any row in its testing set - otherwise it is not possible to calculate the fold's scores! This can happen if there are not enough rows in the dataframe. In this instance, the program will raise an exception telling you your data is invalid.

## How does `CVFrames` cut the cross validation frames?

The way we cut cross validation range is described as follows:
1. We know each of the rows in the dataframe has an index attached to it.
2. We split the indices into `nfolds` sets. Suppose there are 20 rows in the dataframe, `validation_percentage` is 0.2, and `nfolds` is 5. This is how we split the indices:
    ```python
    ranges = numpy.array_split(numpy.arange(dataframe.shape[0]), nfolds)
    # ranges = np.array_split(np.arange(20), 5)
    # ranges = [
    #     array([0, 1, 2, 3]),
    #     array([4, 5, 6, 7]),
    #     array([8, 9, 10, 11]),
    #     array([12, 13, 14, 15]),
    #     array([16, 17, 18, 19])
    # ]
    ```
3. Now that we have `ranges`, a list of 5 sublists, each indicating a set of indices. For the first cross validation fold, here's what we do:
    1. We take the rows in the first sublist as the testing set, the rest will be either be in training set or validation set.
    2. We now have 16 rows that are not in the testing set. To split those into training set and validation set, we use the `sample` function in pandas:
        ```python
        validation_indices = df.sample(frac=0.2)
        ```
        Now that we know which indices should be in the validation set, we can split the frame accordingly.
4. Repeat this process for the rest of the folds.
