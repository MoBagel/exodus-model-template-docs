# Return the results

After we are done training and saving the model, the last step would be returning the results to the user.

```python
        # Create the `Attributes` instance
        attributes = Attributes(cv_scores=cv_scores, holdout_scores=holdout_scores)

        # Create the response, then return it
        response = TrainRespBody(
            name=self.name,
            attributes=attributes,
            model_id=model_info_id,
            hyperparameters=model.get_params(),
        )

        return response
```

Here `cv_scores` and `holdout_scores` are both calculated earlier during the training process.

## The model hyperparameters

Notice there's a field for hyperparameters in `TrainRespBody`. This is to record the machine learning algorithm's hyperparameters, and should be a `Dict`. Most of the machine learning algorithms provide a function to expose the set of hyperparameters, but if it is not the case you should look up the possible settings for the machine learning algorithm, and then put them in this field.
