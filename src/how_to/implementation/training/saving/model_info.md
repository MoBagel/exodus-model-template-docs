# Store the necessary informations

Let's take a look at the declaration of the `ModelInfo` class:

```python
class ModelInfo(BaseModel):
    name: str
    target: Column
    model_id: str
    encoder_ids: Dict[str, str]
    time_components: List[str]
    feature_types: List[Column]

    encrypted_fields: List[str] = Field(
        default=["target", "encoder_ids", "time_components", "feature_types"],
        exclude=True,
    )

    model: Union[LinearRegression, LogisticRegression] = Field(
        default=None, exclude=True
    )
    encoders: Dict[str, LabelEncoder] = Field(default=None, exclude=True)
```

The only ones that are required are `name`, `target`, `model_id`, `encoder_ids`, `time_components`, and `feature_types`. As for the rest:
- `encrypted_fields` describes what you need to encrypt if `encrypted` is set to `true` in `config.ini`. See Appendix C for more info.
- `model` is the actual machine learning model that you just trained. During prediction, you should be using this.
- `encoders` is the actual encoders. During prediction, you should be using this.

Let's get back to the `save` method:

```python
        model_info = cls(
            name=name,
            target=target,
            model_id=model_id,
            encoder_ids=saved_encoders,
            time_components=time_components,
            feature_types=feature_types,
        )
```

Notice how we only fill in the required fields, and left the rest as default.

## What should I include in `ModelInfo`?

Rule of thumb is to include anything you will need for prediction. That could include the following:
- A portion of the training dataframe
- Some more feature engineering artifacts
- Columns that require additional processing

## What should I implement?

The developer of the model algorithm should implement the following:

1. A way to dump the things into an object that can be downloaded (see `dump_to_base64` method)
2. The `delete` method, when you delete a `ModelInfo` you should also remove the relevant machine learning model and feature engineering artifacts
