# Appendix C: How to change MongoDB settings

## Changing MongoDB service port

By default, the MongoDB service is reachable via port `27018`. If you want to change that to something else, say `9487`, modify the `docker-compose.yml`:

```yml
services:
  mongo:
    restart: always
    build:
      context: ./mongo
      dockerfile: Dockerfile
    ports:
      - "27018:27017" # Change this to "9487:27017"
```

If you want to run tests using your own port, modify `tests/__init__.py`, and change it from `27018` to `9487`.

## Changing other MongoDB configurations

All of the configuration values are in `config.ini`.

## Encryption

In order to properly encrypt things, several things are required:
- `encrypted` field in `config.ini` should be `true`
- `encryption_key` in `config.ini` should be a 96 bytes long string

The encryption key will be stored in MongoDB, in a collection named `__keyVault` in the database specified in `config.ini`. There should be none or only one document in the collection.
