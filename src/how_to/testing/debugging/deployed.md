# Debugging the model algorithm with actual requests

Right now we haven't figured out a way to debug FastAPI applications in a container, so to use a debugger to debug the application you need to the following:
1. Start MongoDB service
2. In `app.py`, modify the declaration of `model_algorithm` variable to the following:
    ```python
    model_algorithm = ModelAlgorithm(host="localhost", port="27018")
    ```
3. Run the FastAPI application
    - In VSCode: insert the following code snippet to `.vscode/launch.json` (can be opened via `Show All Commands` > `Open 'launch.json'`)
        ```json
        {
            "name": "Python: FastAPI",
            "type": "python",
            "request": "launch",
            "module": "uvicorn",
            "args": [
                "exodus_model_template.app:app"
            ],
            "jinja": true,
            "justMyCode": true
        }
        ```
    - In command line: run the app via the following command:
        ```bash
        poetry run python -m pdb exodus_model_template/app.py
        ```

## Debug the deployed, containerized model algorithm

TODO
