# Using the landing page

See `Run the model template`.

## What if I don't want to use port 3000 for the webapp?

In `docker-compose.yml`:
```yaml
services:
# ... snipped ...
  web_app:
    ports:
      - "3000:3000" # Change this to "{SOME_PORT}:3000"
```

Then in `exodus_model_template/app.py`:
```python
# ... snipped ...
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"], # Change this to "http://localhost:{SOME_PORT}"
```

## Can I use the webapp to access a model algorithm that's not running on localhost?

This is currently not supported.
