# Saving scikit-learn stuff

The maximum size limit for a MongoDB document is 4MB. Unfortunately, it is not unlikely that your machine learning will exceed that limit. To handle this behavior, we can store our machine learning and label encoders in GridFS, a component of MongoDB that is designed to specifically handel this scenario.

```python
def save_sklearn_stuff(stuff: Any, gridfs: GridFS) -> str:
    return str(gridfs.put(pickle.dumps(stuff)))
```

We use this function to store `scikit-learn` artifacts, namely the machine learning model and the label encoders. The function is actually very simple:
1. Dumps the thing into a byte sequence using `pickle`, the de-facto default serializer for Python
2. Stores the byte sequence into GridFS, and retrieve an `ObjectId`
3. Converts that `ObjectId` into a string, for the former does not play well with built-in Python classes

Notice there is a `FIXME` beneath the comments, this is to warn you that by pickling an artifact, you are also including its dependencies such as libraries, versions, and so on. This makes it virtually impossible for backward compatibility if you ever plan to upgrade a package that you've installed. Luckily it is not really something you should do that often, if at all. Still, if you find yourself in need of upgrading a package, and you've pickled something generate from that package, chances are you are better off creating a new model that makes use of the newer package.
