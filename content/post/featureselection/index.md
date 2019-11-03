+++
title = "Feature Selection"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Saavas writes on DataDive that there are two reasons to use feature selection

> 1. Optimize Accuracy: Reducing the number of features, to reduce overfitting and improve the generalization of models.
2. Interpret/Understand: To gain a better understanding of the features and their relationship to the response variables.

### Plot a list of scatterplots of numeric features versus target

    target = 'price'
    numeric_columns = df.select_dtypes(include='number').columns
    for feature in numeric_columns.drop(target):
        sns.scatterplot(x=feature, y=target, data=df, alpha=0.1)
        plt.show()

### View coefficients of high impact features

    coef = pd.Series(model.coef_, index = X_train.columns)
    
    imp_coef = pd.concat([coef.sort_values().head(10),
                         coef.sort_values().tail(10)])
    
    imp_coef.plot(kind = "barh")
    plt.title("Important Coefficients in the Model")

### Model Based Ranking

One method to investigate features is to build a separate predictive model for each individual feature. In a sense the correlation matrix does this (w/linear regression). 

Here is an implementation that uses a random forest model:

    from sklearn.cross_validation import cross_val_score, ShuffleSplit
    from sklearn.datasets import load_boston
    from sklearn.ensemble import RandomForestRegressor
     
    #Load boston housing dataset as an example
    boston = load_boston()
    X = boston["data"]
    Y = boston["target"]
    names = boston["feature_names"]
     
    rf = RandomForestRegressor(n_estimators=20, max_depth=4)
    scores = []
    for i in range(X.shape[1]):
         score = cross_val_score(rf, X[:, i:i+1], Y, scoring="r2",
                                  cv=ShuffleSplit(len(X), 3, .3))
         scores.append((round(np.mean(score), 3), names[i]))
    print sorted(scores, reverse=True)

Source: [Data Dive](http://blog.datadive.net/selecting-good-features-part-i-univariate-selection/)

### Stability Selection

"Apply a feature selection algorithm on different subsets of data and with different subsets of features. After repeating the process a number of times, the selection results can be aggregated, for example by checking how many times a feature ended up being selected as important when it was in an inspected feature subset." 

    from sklearn.linear_model import RandomizedLasso
    from sklearn.datasets import load_boston
    boston = load_boston()
     
    #using the Boston housing data. 
    #Data gets scaled automatically by sklearn's implementation
    X = boston["data"]
    Y = boston["target"]
    names = boston["feature_names"]
     
    rlasso = RandomizedLasso(alpha=0.025)
    rlasso.fit(X, Y)
     
    print "Features sorted by their score:"
    print sorted(zip(map(lambda x: round(x, 4), rlasso.scores_), 
                     names), reverse=True)

[](http://blog.datadive.net/selecting-good-features-part-iv-stability-selection-rfe-and-everything-side-by-side/)

### Residual Plots

Residual plots can help to uncover features which may benefit from further adjustment (e.g., scaling). In particular, it is helpful to look for features with residual plots that display bowing.

    for feature in features:
        sns.residplot(X[feature], y, lowess=True, line_kws=dict(color='r'))
        plt.show()

### Custom Targets

If the target is derived from features in the dataset you cannot use those features in the model. 

Also have to watch out for features that our essentially targets (e.g. alive and survived)

**Have to be careful when touching regularizing data/PCA before splitting into train and test sets**