# Extract unique labels

In order to calculate cross validation scores properly for classification problems, we need to find out the total amount of categorical values in the target column. This is done in the below code snippet:

```python
        # the unique labels in the target column;
        # used for calculating scores
        labels = pd.unique(training_df[request.target.name])
```
