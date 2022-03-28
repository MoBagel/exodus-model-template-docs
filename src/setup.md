# Setup the model template

The model template comes with a fully functional model that can do training and predicting, albeit with rather poor predicting capability.
To setup the model template, several steps are necessary:

1. Renaming the model template package
2. Installing dependency libraries

## Renaming the package

When you get the package, it will be named as `exodus_model_template`, which is most definitely not the
name for the model algorithm you are going to build. To rename the entire package, run the renaming script:
```bash
python scripts/rename.py [NAME_OF_THE_MODEL]
```

## Installing dependency libraries

To install the dependency libraries, enter the following command (assuming you've already installed the packages specified in the `Dependencies` section):
```bash
poetry install --no-root
```
This command will install all the dependency libraries for you.

### The libraies

The model algorithm template depends on several libraries, the ones that are used extensively are:
- [pandas](https://pandas.pydata.org/): the de-facto go-to data analysis package for Python. We are using v1.4.0.
  - Note that `pandas` artifacts such as `DataFrame` are not backward compatible. Think it through if you want to bump its version.
- [numpy](https://numpy.org/): Python's math libaray. We are using v1.21.0.
- [scikit-learn](https://scikit-learn.org/): the machine learning Swiss knife library. Currently only the `LabelEncoder` class is used. We are using v1.0.2.
  - Note that `scikit-learn` artifacts such as `LabelEncoder` are not backward compatible. Think it through if you want to bump its version.
- [pymongo](https://pymongo.readthedocs.io/en/stable/): the MongoDB Python client library.
- [jinja2](https://jinja.palletsprojects.com/): a templating tool for Python. Used for generating migration scripts.
- [fastapi](https://fastapi.tiangolo.com/): the web framework we chose to run the model algorithm service on. We are using v0.14.1.
- [pydantic](https://pydantic-docs.helpmanual.io/): the data validation library for Python classes. We are using v.1.9.0.
- [exodusutils](https://pypi.org/project/exodusutils/): the utility functions for Exodus. The version of this package is subject to change.
