+++
title = "Ridge Regression"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
OLS minimizes the sum of squared error which emphasizes outliers. **Regularization adds bias to reduce variance.** 

> Variance is when a model is sensitive to noise in the training data, and generalizes poorly — overfitting!

> Bias is when a model isn't recognizing relations between features and output — underfitting!

Ridge regression minimizes the sum of squared error plus the sum of the squared coefficients (time a scaling parameter lambda). Essentially the loss function penalizes large coefficients. 

This means that the model seeks to avoid large coefficients, returning flatter results. For this reason, the features need to all be scaled (to avoid some features disproportionately being penalized).

    # Data should be scaled to best utilize Ridge Regression
    from sklearn.linear_model import Ridge
    
    ridge_reg = Ridge().fit(X, y)
    mean_squared_error(y, ridge_reg.predict(X))

Adjusting Ridge Regression's alpha parameter is the most common way to calibrate a ridge regression model. 

> The alpha parameter corresponds to the weight being given to the extra penalty being calculated by the scaling parameter (aka lambda)

A Ridge Regression with alpha=0 will be the same as a linear regression model. As alpha approaches infinity the model will output an increasing number of coefficients as zero until it is just a flat model with the intercept as the mean of the target.

Visualize a model's MSE across different alpha values:

    from sklearn.linear_model import Ridge
    
    alphas = []
    mses = []
    
    for alpha in range(0, 200, 1):
        ridge_reg_split = Ridge(alpha=alpha).fit(X_train, y_train)
        mse = mean_squared_error(y_test, ridge_reg_split.predict(X_test))
        print(alpha, mse)
        alphas.append(alpha)
        mses.append(mse)
    
    from matplotlib.pyplot import scatter
    scatter(alphas, mses);

## Regularization

> Regularization means increasing model bias by 'watering down' coefficients with a penalty typically based on some sort of distance metric, and thus reducing variance (overfitting the model).

Another way to think of regularization is penalizing higher degree polynomials (coefficients) to ensure they are only selected if they reduce the error significantly.

"Regularization is a method for adding additional constraints or penalty to a model, with the goal of preventing overfitting and improving generalization." ([Data Dive](https://blog.datadive.net/selecting-good-features-part-ii-linear-models-and-regularization/))

From Quora: 

> The goal of machine learning is to model the pattern and ignore the noise. Anytime an algorithm is trying to fit the noise in addition to the pattern, it is overfitting.

Ridge Regression uses L2 regularization (due to the squaring of the slopes being summed). This corresponds to a measure that can intuitively be thought of as straight distance.

L1 regularization uses 'Manhattan distance' (e.g., following the grid). L1 regularization is used by Lasso models.

# Lasso Regression

Unlike Ridge, Lasso regression takes into account the magnitude of the coefficients instead of the square. Thus, Lasso can lead to coefficients = 0. Thus, lasso regression can help with feature selection. 

    from sklearn.linear_model import LassoCV
    
    pipe = make_pipeline(StandardScaler(), LassoCV(cv=3))
    pipe.fit(X_train, y_train)
    lasso = pipe.named_steps['lassocv']
    print('Lasso Linear Model, alpha value optimized with cross validation:', lasso.alpha_)

For classification problems, logistic regression can emulate a lasso regression if the L1 penalty parameter is enabled.

# Lasso Vs. Ridge

> Lasso produces sparse solutions and as such is very useful selecting a strong subset of features for improving model performance. Ridge regression on the other hand can be used for data interpretation due to its stability and the fact that useful features tend to have non-zero coefficients. ([Data Dive](https://blog.datadive.net/selecting-good-features-part-ii-linear-models-and-regularization/))

Ridge regression is more stable (smaller changes in coefficients) as data changes. This makes Ridge regression more attractive for feature interpretation.