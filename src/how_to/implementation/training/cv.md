# Calculate cross validation scores

Next, we calculate the cross validation scores for the model. The procedure is as follows:
1. Cut the training dataframe into several sub-frames
2. For each of those sub-frames, create a machine learning model
3. For each of those trained machine learning models, calculate their performance by comparing its predictions to another sub-frame

The corresponding code is the following snippet:

```python
        # Cut the training dataframe into cross-validation frames
        cv_frames = CVFrames.iid(
            training_df, request.folds, request.get_validation_data()
        )

        # Use the cross-validation frames to calculate the cv scores
        folds = [do_cv_fold(cv_frame, request.target, labels) for cv_frame in cv_frames.frames]
        cv_scores = CVScores(folds=folds)
```

We have provided several helper functions to make this a lot easier for you: the sub-frames splitting and the score calculations are all done by helpers in the `exodusutils` function! The only place you need modifying is in `do_cv_fold` function:

```python
def do_cv_fold(train_frames: TrainFrames, target: Column, labels: np.ndarray):
    """
    Does a cross validation fold. Includes training a cross validation model and calculating the score for the model.

    Parameters
    ----------
    train_frames : TrainFrames
        The frames for this cross validation fold.
    target : Column
        The target column.
    labels : np.ndarray
        The unique classes that are involved in the classification problem.

    Returns
    -------
    scores
        The scores for this cross validation fold.
    """
    cv_model = train_model(train_frames, target)
    if train_frames.test is None:
        raise ExodusBadRequest(detail=f"Cannot create test frame for this fold")
    return calculate_score(cv_model, train_frames.test, target, labels)
```

This is a rather simple function, but there are 2 methods called here: `train_model` and `calculate_score`, the former correspond to the training model part, while the latter the score calculation part. We will go over them in the following sections.
