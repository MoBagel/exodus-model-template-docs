# Piecing it together

Here's the `predict` method in its entirety (sans comments):

```python
    def predict(self, request: PredictReqBody) -> PredictRespBody:

        # LOADING #

        model_info = ModelInfo.load(request.model_id, self.collection, self.gridfs)

        # SANITIZING #

        original_df = request.get_prediction_df(model_info.feature_types)
        sanitized_df = remove_invalid_values(original_df)

        # APPLYING FEATURE ENGINEERING #

        df = apply_feature_engineering(sanitized_df, model_info.encoders)

        # PREDICTING #

        features = df.loc[:, df.columns != model_info.target.name]
        predicted = model_info.model.predict(features)

        # FORMATING RESULTS #

        sanitized_df[PREDICTION_COLNAME] = predicted

        if model_info.target.data_type != DataType.double:
            if not isinstance(model_info.model, LogisticRegression):
                raise RuntimeError(
                    f"Got {model_info.model.__class__} when it should be LogisticRegression"
                )
            classes = list(model_info.model.classes_)
            predict_proba = pd.DataFrame(
                model_info.model.predict_proba(features), columns=classes
            )
            sanitized_df = pd.concat([sanitized_df, predict_proba], axis=1).reset_index(
                drop=True
            )
        else:
            classes = list()

        columns_to_drop = [
            c
            for c in list(sanitized_df.columns)
            if c not in request.keep_columns
            and c != PREDICTION_COLNAME
            and c not in classes
        ]

        # PREPARING RESPONSE #

        results: List[Dict[str, Any]] = json.loads(
            str(sanitized_df.drop(columns=columns_to_drop).to_json(orient="records"))
        )

        response = PredictRespBody(prediction=results)
        return response
```
