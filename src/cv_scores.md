# Appendix B: How cross validation scores are calculated

To create the frames for cross validation, `exodusutils` library provides a helper class called `CVFrames`:
```python
frames: List[TrainFrame] = CVFrames(df, nfolds, validation_df).frames
```

Here, `df` is the training dataframe, `nfolds` is the number of folds you want for the cross validation, and `validation_df` represents the validation dataframe, which is optional.

In the return type, a `TrainFrame` is essentially a 3-tuple comprised of three pandas `DataFrame`s. The first is the dataframe that will be used to train the cross validation model, the second used as the validation set during training (this is the `validation_df` argument you passed to `CVFrames`), the third as the testing set for calculating the cross validation score.

Note that a cross validation fold is invalid if it does not have any row in its testing set - otherwise it is not possible to calculate the fold's scores! This can happen if there are not enough rows in the dataframe. In this instance, the program will raise an exception telling you your data is invalid.

## How does `CVFrames` cut the cross validation frames?

The way we cut cross validation range is described as follows:
1. We know each of the rows in the dataframe has an index attached to it.
2. We split the indices into `nfolds` sets. Suppose there are 20 rows in the dataframe, and `nfolds` is 5, we calculate `row_index % fold` to see which rows goes to which fold. To demonstrate this, below is an example on how we split the indices:
    ```python
    fold_indices = [
      [0, 5, 10, 15],
      [1, 6, 11, 16],
      [2, 7, 12, 17],
      [3, 8, 13, 18],
      [4, 9, 14, 19]
    ]
    ```
3. Now that we have `fold_indices`, a list of 5 sublists, each indicating a set of indices. For the first cross validation fold, we take the rows in the first sublist as the testing set, the rest will be either be in training set or validation set.
4. Repeat this process for the rest of the folds.
