# Exceptions

Instead throwing `ValueError` or `HTTPException`, you can use the following:

If the input is malformed:
- `ExodusBadRequest`
- `ExodusForbidden`
- `ExodusMethodNotAllowed`
- `ExodusNotFound`

If the model algorithm is supposed to fail, throw `ExodusError` instead of other exceptions.
