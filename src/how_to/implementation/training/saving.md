# Save the model

Let's take a look at the following code snippet in the `train_iid` method:
```python
        # Save the model info into MongoDB
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
```

And in `model_info.py`, the `save` method for `ModelInfo` is defined as:

```python
    @classmethod
    def save(
        cls,
        name: str,
        target: Column,
        model: Union[LinearRegression, LogisticRegression],
        encoders: Dict[str, LabelEncoder],
        time_components: List[str],
        feature_types: List[Column],
        mongo: MongoInstance,
        gridfs: GridFS,
        collection: Collection,
    ) -> str:

        model_id = save_sklearn_stuff(model, gridfs)
        saved_encoders = {k: save_sklearn_stuff(v, gridfs) for k, v in encoders.items()}

        model_info = cls(
            name=name,
            target=target,
            model_id=model_id,
            encoder_ids=saved_encoders,
            time_components=time_components,
            feature_types=feature_types,
        )

        doc = model_info.dict()
        for field in model_info.encrypted_fields:
            if doc.get(field) is None:
                raise ExodusError(f'field = "{field}" is not a valid key for ModelInfo')
            doc[field] = mongo.encrypt(doc[field])
        return str(collection.insert_one(doc).inserted_id)
```

The `save` method returns a stringified `ObjectId`, which you should pass back to the user so that they can identify the model that was just trained.

The `save` method can be split into 3 parts:
1. Saving `scikit-learn` stuff
2. Creating a `ModelInfo` instance
3. Encrypting the `ModelInfo` instance and storing it into MongoDB
