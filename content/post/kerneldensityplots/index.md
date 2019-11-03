+++
title = "Kernel Density Plots"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
# Kernel Density

> A kernel density estimate plot shows the distribution of a single variable and can be thought of as a smoothed histogram. It is computed by computing a kernel, usually a Gaussian, at each data point and then averaging all the individual kernels to develop a single smooth curve
```
    import seaborn as sns
    
    plt.figure(figsize = (10, 8))
    
    # KDE plot of loans that were repaid on time
    sns.kdeplot(app_train.loc[app_train['TARGET'] == 0, 'DAYS_BIRTH'] / 365, label = 'target == 0')
    
    # KDE plot of loans which were not repaid on time
    sns.kdeplot(app_train.loc[app_train['TARGET'] == 1, 'DAYS_BIRTH'] / 365, label = 'target == 1')
    
    # Labeling of plot
    plt.xlabel('Age (years)'); plt.ylabel('Density'); plt.title('Distribution of Ages');
```
[Start Here: A Gentle Introduction](https://www.kaggle.com/willkoehrsen/start-here-a-gentle-introduction)