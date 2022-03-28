# Migrating the ModelInfo

If you modify the `ModelInfo` of a model algorithm that's been published before, it is then your responsibility to make sure it is consistent between different versions. To ensure this, you need to write a migration script.

## Creating a migration script

```bash
poetry run python scripts/make_migration_script.py [SCRIPT NAME]
```

Let's say the name is `foo`, this is what would happen:

```bash
❯ poetry run python scripts/make_migration_script.py foo
Created: exodus_model_template/migrations/Migration_1646728080972_foo.py
Updated: exodus_model_template/migrations/all_migrations.py

```

A migration script called `exodus_model_template/migrations/Migration_1646728080972_foo.py` has been generated.

## Implementing the migration script

Suppose your old `ModelInfo` is defined like this:
```python
class ModelInfo(BaseModel):
    foo: str
    bar: int
```

And you want to change it to this:
```python
class ModelInfo(BaseModel):
    foo: str
    bar: int
    baz: str
```
where `baz` should be `foo` concatenated with a stringified `bar`.

In the migration script, there are mainly two methods: `up` and `down`:
```python
class Migration(BaseMigration):
    def __init__(self, timestamp: int = 1646728080972, name: str = "foo") -> None:
        super().__init__(timestamp, name)

    def up(self, collection: Collection) -> None:
        # TODO implement this method
        pass

    def down(self, collection: Collection) -> None:
        # TODO implement this method
        pass
```

For `up`, you want to find all the `ModelInfo` instances that don't have a field called `baz`. An examplary implementation might be this:
```python
    def up(self, collection: Collection) -> None:
        for doc in collection.find({"baz": {"$exists": False}}):
            collection.update_one(filter={"_id": doc["_id"]}, update={"$set": {"baz": doc["foo"] + str(doc["bar"])}})
```

As for `down`, you do the exact opposite and unset the `baz` field:
```python
    def down(self, collection: Collection) -> None:
        for doc in collection.find({"baz": {"$exists": True}}):
            collection.update_one(filter={"_id": doc["_id"]}, update={"$unset": {"baz": ""}})
```

## Invoking the migration scripts

It is Exodus main that will be in charge of migrating, however if you want to test your script manually, do this:
```bash
curl -X POST "http://{MODEL ALGORITHM IP}/migrate" -d '{"action":"up"}' -H "Content-Type: application/json"
```

Change `"up"` to `"down"` if that's what you want to test.
