+++
title = "Standardizing Data"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++

Standardizing data is critical as many predictive algorithms rely upon the assumption that data is more or less normally distributed (gaussian). 

In practice, the shape of the distribution is often ignored, but the data is transformed to center by removing the mean value of each feature and scaling it by dividing features by their standard deviation. 

### Standard Scaler

Sklearn's standard scaler computes the mean and standard deviation on a training set so as to later apply that transformation to the testing set. 

    from sklearn.preprocessing import StandardScaler
    
    scaler = StandardScaler()
    X_train_scaled = scaler.fit_transform(X_train_numeric)
    X_val_scaled   = scaler.transform(X_val_numeric)

[4.3. Preprocessing data - scikit-learn 0.20.3 documentation](https://scikit-learn.org/stable/modules/preprocessing.html)