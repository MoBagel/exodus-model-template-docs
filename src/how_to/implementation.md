# Implementing the model algorithm service

This section will walk you through implementing the model algorithm service.

The model algorithm template already contains a working example. For the rest of the documentation, we will be using that to demonstrate the process.

The only files you should modify in theory are those two:
- `model_algorithm.py`, which contains the actual model training, persisting and predicting.
- `model_info.py`, which defines the schema of the model you are persisting into MongoDB.
