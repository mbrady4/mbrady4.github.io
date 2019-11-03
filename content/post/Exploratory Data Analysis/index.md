+++
title = "Exploratory Data Analysis Process"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
###Process for exploratory analysis: 

1. **Understand the problem.** Look at each variable and do a logic-driven analysis to estimate meaning and importance to problem. 
    - Consider creating an excel sheet with columns for:
        - *Variable name*
        - *Type* (e.g., numerical, categorical)
        - *Segment* (create sub-groups of features that are related to each other)
        - *Expectation* (e.g., Low/Medium/High impact on target)
        - *Comments*
2. **Study the target.** Understand the independent variable.
    - Check descriptive statistics

            ```df_train['Target'].describe()```

    - Understand distribution w/histogram (Is distribution normal?)
        - Skew, Kurtosis
```
            print("Skewness: %f" % df_train['Target'].skew())
            print("Kurtosis: %f" % df_train['Target'].kurt())
            sns.distplot(df_train['Target']);
```
3. **Multivariate study.** Understand how the dependent variables and independent variables relate.
    - Individual *Continuous features:* Use scatterplots with target as y-axis. Note, can be helpful to keep the same y-limit across plots —> use ylim=(0,max)
```
            var = 'Feature'
            data = pd.concat([df_train['Target'], df_train[var]], axis=1)
            data.plot.scatter(x=var, y='Target');
```
    - Individual *Categorical features:* Use box plots to visualize relationship with target.
```
            var = 'Feature'
            data = pd.concat([df_train['Target'], df_train[var]], axis=1)
            f, ax = plt.subplots(figsize=(8, 6))
            fig = sns.boxplot(x=var, y="Target", data=data)
            fig.axis(ymin=0, ymax=800000);
```
    - Correlation Matrix: Use a correlation matrix to identify collinear relationships between features and the features most correlated with the target
```
        import seaborn as sns
        import matplotlib.pyplot as plt
        
        corrmat = df_train.corr()
        f, ax = plt.subplots(figsize=(12, 9))
        sns.heatmap(corrmat, vmax=.8, annot=True, square=True);

        #Get the k most correlated features with the target
        k = 10 #number of variables for heatmap
        # corrmat initialized in above snippet
        cols = corrmat.nlargest(k, 'Target').index
        cm = np.corrcoef(df_train[cols].values.T)
        sns.set(font_scale=1.25)
        hm = sns.heatmap(cm, cbar=True, annot=True, square=True, fmt='.2f', annot_kws={'size': 10}, yticklabels=cols.values, xticklabels=cols.values)
        plt.show()
```
    - Alternatively: List the features with the highest correlation to the target
```
        print("Find most important features relative to target")
        corr = train.corr()
        corr.sort_values(["Target"], ascending = False, inplace = True)
        print(corr.SalePrice)
```
    One note is that the Pearson correlation is only sensitive to linear relationships—thus another type of relationship (e.g., exponential) between variables will not be surfaced. 

    - Create a pairplot: Include only the highest potential features
```
        sns.set()
        cols = ['SalePrice', 'OverallQual', 'GrLivArea', 'GarageCars', 'TotalBsmtSF', 'FullBath', 'YearBuilt']
        sns.pairplot(df_train[cols], height = 2.5)
        plt.show();
```
4. **Basic Cleaning.** Clean the dataset, handling missing data, outliers, and categorical variables
    - How prevalent is the missing data? Is the missing data random or does it have a pattern?
```
        # Create a temporary df to summarize missing values
        total = df_train.isnull().sum().sort_values(ascending=False)
        percent = (df_train.isnull().sum()/df_train.isnull().count()).sort_values(ascending=False)
        missing_data = pd.concat([total, percent], axis=1, keys=['Total', 'Percent'])
```
5. **Test Assumptions.** Check if the data meets assumptions required by our multivariate techniques
    - Does data need to be standardized?