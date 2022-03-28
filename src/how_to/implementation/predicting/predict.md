# Predict

After we've sanitized and applied feature engineering, it is finally time to predict with our model. Note that during cross validation we've already predicted using a calculated machine learning model though, so here it's more or less just repeating what you did back then.

```python
        features = df.loc[:, df.columns != model_info.target.name]
        predicted = model_info.model.predict(features)
```

Here we extract the features we are using, then predict using the selected features.
