# Requests

## `TrainIIDReqBody`

Fields:
- `training_data`: the raw bytes representing the training data. Is a CSV file as a sting. Should not be accessed directly.
- `feature_types`: the columns in the training data.
- `target`: the target `Column`.
- `folds`: the number of folds during cross validation.
- `validation_percentage`: the percentage of rows to be used as validation set during training.
- `holdout_data`: the optional raw bytes representing the holdout data. Should not be accessed directly.

Methods:
- `get_feature_names`: returns the names of the columns.
- `get_training_df`: returns the parsed pandas `DataFrame` generated from the bytes in `training_data`. Note that the data types of the columns will correspond to the ones specified in `feature_types`.
- `get_holdout_data`: returns the parsed pandas `DataFrame` generated from the bytes in `holdout_data`, or `None` if there's no holdout data.

## `PredictReqBody`

Fields:
- `model_id`: the ID of the model you want to predict with.
- `data`: the raw bytes representing the prediction data. Is a JSON string. Should not be accessed directly.
- `threshold`: the threshold for classification predictions.
- `keep_columns`: the columns to keep in the prediction results.

Methods:
- `get_prediction_df`: takes in a list of `Column`s, and returns a pandas `DataFrame` representing the prediction data. The list of `Column` should be stored into MongoDB once training is completed.
