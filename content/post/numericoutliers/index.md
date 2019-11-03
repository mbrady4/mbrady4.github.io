+++
title = "Visually detecting and flagging numeric outliers"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Michael W. Brady"]
+++
### Detect outliers with histograms and .describe()

    df['Feature'].describe()
    
    df['Feature'].plot.hist(title = 'Feature Histogram');
    plt.xlabel('Feature');

### Create a binary column to indicate if an observation is an outlier

    Create an anomalous flag column
    app_train['DAYS_EMPLOYED_ANOM'] = app_train["DAYS_EMPLOYED"] == 365243
    
    # Replace the anomalous values with nan
    app_train['DAYS_EMPLOYED'].replace({365243: np.nan}, inplace = True)

[Start Here: A Gentle Introduction](https://www.kaggle.com/willkoehrsen/start-here-a-gentle-introduction)