# Adding or removing dependency libraries

Before you start creating your own model, you need to install some machine learning libraries that contain the algorithms you will be using. `poetry`, the package manager we are using for this project, actually comes with a neat virtual environment right out of the box, so you do not need to worry about messing up your host environment.

Take our model algorithm template for example. We decided to use [`Linear Regression`](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html) and [`Logistic Regression`](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) as our machine learning algorithm (there are 2 of them because `Linear Regression` only handles regression problems, while `Logistic Regression` only handles categorical ones). These algorithms can be found as a part of the [`scikit-learn`](https://scikit-learn.org/stable/index.html) package, so that's what we installed for this example.

## Adding a dependency library

To install the `scikit-learn` package and add it to the dependency libraries, simply do the following:
```bash
poetry add scikit-learn
```

The above command does not specify a version for the package, so `poetry` will just use the newest one that is compatible with all the other packages we've listed as dependencies. If you want to use the latest version:
```bash
poetry add scikit-learn@latest
```

Or if you want to stick to a specific version:
```bash
poetry add scikit-learn@1.0.2
```

## Removing a dependency library

Suppose instead of those machine learning algorithms, you decided that you want to use something else. The `scikit-learn` package contains the `LabelEncoder` class, which we based one of our feature engineering methods (you will know more about this later!) on, so it does not make sense to remove the library from your dependencies.

**The below is only an example and should not actually be run**, but suppose you want to remove `scikit-learn`:
```bash
poetry remove scikit-learn
```
