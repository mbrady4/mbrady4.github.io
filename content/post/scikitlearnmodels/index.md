+++
title = "Sklearn model building process"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
The steps the general steps to build a model are:

1. Import an estimator class (e.g., LinearRegression) from sklearn
2. Instantiate the class and specify model hyperparameters
3. Arrange data into a features matrix (or DataFrame) and create a target array (or Series)
4. Fit the model to the data with .fit()
5. Apply the model to new data with .predict() or .transform()

    #1
    from sklearn.linear_model import LinearRegression
    #2
    model = LinearRegression()
    #3
    X_train, X_test, y_train, y_test = train_test_split(X, y, train_size=0.80, test_size=0.20, random_state=42)
    #4
    model.fit(X_train, y_train)
    #5
    y_pred = model.predict(X_test)