# Predicting with a trained model

Once we have successfully trained and stored a model, next we will need to predict something from this model. The general workflow is as follows:
1. Load the model from MongoDB
2. Sanitize the prediction input
3. Apply the feature engineering steps
4. Predict
5. Format the predicted results
