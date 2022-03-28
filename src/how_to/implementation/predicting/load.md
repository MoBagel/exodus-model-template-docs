# Load a model

In order to properly predict with a previously trained model, we need to first load the saved model from MongoDB.

Basically, you want to do the inverse of what you did during training: if you stored some `LabelEncoder`s into GridFS, here you need to extract them, and so on.

```python
    @classmethod
    def load(cls, model_id: str, collection: Collection, gridfs: GridFS):
        obj = collection.find_one(filter=identify(model_id))
        if not obj:
            raise ExodusNotFound(f"No model found with id = {model_id}")
        model_info = cls.parse_obj(obj)
        model_info.encoders = {
            k: load_sklearn_stuff(v, gridfs) for k, v in model_info.encoder_ids.items()
        }
        model_info.model = load_sklearn_stuff(model_info.model_id, gridfs)

        return model_info
```

Compare the above with the code snippet where we store the machine learning model and encoders into GridFS:
```python
        model_id = save_sklearn_stuff(model, self.gridfs)
        saved_encoders = {k: save_sklearn_stuff(v, self.gridfs) for k, v in encoders.items()}
```

Remember we had fields `model` and `encoders` in our `ModelInfo`? During `load`, we actually store those in the resulting `ModelInfo`, so we can guarantee that after loading a `ModelInfo`, it is safe to access the `model`, which is the machine learning model, and the `encoders`, which are the label encoders.
