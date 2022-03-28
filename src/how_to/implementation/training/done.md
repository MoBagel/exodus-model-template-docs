# Piecing it together

Here's the `train_iid` method in its entirety (sans comments):

```python
    def train_iid(self, request: TrainIIDReqBody) -> TrainRespBody:

        # SANITIZING #

        sanitized_df = remove_invalid_values(request.get_training_df())
        raw_holdout_df = request.get_holdout_data()
        if raw_holdout_df is not None:
            sanitized_holdout = remove_invalid_values(raw_holdout_df)
        else:
            sanitized_holdout = None

        # FEATURE ENGINEERING #

        training_df, holdout_df, encoders, components = feature_engineering(
            sanitized_df, sanitized_holdout
        )

        labels = pd.unique(training_df[request.target.name])

        # CROSS VALIDATION #

        cv_frames = CVFrames.iid(
            training_df, request.folds, request.validation_percentage
        )

        folds = [
            do_cv_fold(cv_frame, request.target, labels)
            for cv_frame in cv_frames.frames
        ]
        cv_scores = CVScores(folds=folds)

        # TRAIN THE MODEL #

        train_frames = TrainFrames.iid_without_test(
            training_df, request.validation_percentage
        )
        model = train_model(train_frames, request.target)

        # HOLDOUT #

        if holdout_df is not None:
            holdout_scores = calculate_score(model, holdout_df, request.target, labels)
        else:
            holdout_scores = None

        # SAVING #

        model_info_id = ModelInfo.save(
            name=self.name,
            target=request.target,
            model=model,
            encoders=encoders,
            time_components=components,
            feature_types=request.feature_types,
            mongo=self.mongo,
            gridfs=self.gridfs,
            collection=self.collection,
        )

        # PREPARING RESPONSE #

        attributes = Attributes(cv_scores=cv_scores, holdout_scores=holdout_scores)

        response = TrainRespBody(
            name=self.name,
            attributes=attributes,
            model_id=model_info_id,
            hyperparameters=model.get_params(),
        )

        return response
```

Several things to note here:
1. For the final machine learning model, we do not need to calculate its score. Therefore we use the helper class `TrainFrames`, and invoke its method `iid_without_test` to generate the training frame and the validation frame.
2. Remember in `do_cv_fold` we make use of the methods `train_model` and `calculate_score`? After we have calculated the cross validation scores, we can reuse those methods to calculate the final machine learning model, and calculate the holdout scores.

Now, we have successfully trained our machine learning model, and a set of scores to evaluate this model. In the following section we will see how persisting this model is possible.
