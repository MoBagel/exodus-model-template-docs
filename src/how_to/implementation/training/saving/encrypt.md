# Encrypting & saving the ModelInfo

The `MongoInstance` class contains a method that encrypts values for you automatically, so encryption becomes almost trivial:

```python
        doc = model_info.dict()
        for field in model_info.encrypted_fields:
            if doc.get(field) is None:
                raise ExodusError(f'field = "{field}" is not a valid key for ModelInfo')
            doc[field] = mongo.encrypt(doc[field])
```

You turn the `ModelInfo` into a dictionary (this is the document we will be storing into MongoDB), and replace the fields you want to encrypt with the encrypted value.

If there's a key in the `encrypted_fields` variable that does not exist in the fields of `ModelInfo`, an exception will be raised.

To save the model, we insert it into the MongoDB collection corresponding to the model algorithm, and return the stringified `ObjectId`.

```pythons
        return str(collection.insert_one(doc).inserted_id)
```
