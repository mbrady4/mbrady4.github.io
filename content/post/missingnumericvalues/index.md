+++
title = "Handling Missing Numeric Values"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Michael W. Brady"]
+++
# Missing Numeric Values

The first step should always be to understand the variable and hypothesize why the value might be missing (e.g., is there a clear explanation?). 

We have a few options to deal with missing values: 

1. **Drop columns with missing values:** Only recommended if the feature has many (subjective) missing values. Be sure to drop columns from both the train and test datasets
2. **Impute values with feature average:** Imputation fills in the missing values with some number (typically the feature mean). 

    from sklearn.impute import SimpleImputer
    my_imputer = SimpleImputer()
    data_with_imputed_values = my_imputer.fit_transform(original_data)

Alternatively, one can simply plot use the mean of the column and fillna():

    all_data = all_data.fillna(all_data.mean())

3. **Impute values and add columns to indicate imputation:** Creating an additional binary column to indicate if a value was imputed can provide additional information to the model (improving results). 

    # make copy to avoid changing original data (when Imputing)
    new_data = original_data.copy()
    
    # make new columns indicating what will be imputed
    cols_with_missing = (col for col in new_data.columns 
                                     if new_data[col].isnull().any())
    for col in cols_with_missing:
        new_data[col + '_was_missing'] = new_data[col].isnull()
    
    # Imputation
    my_imputer = SimpleImputer()
    new_data = pd.DataFrame(my_imputer.fit_transform(new_data))
    new_data.columns = original_data.columns

Source: 

[Handling Missing Values](https://www.kaggle.com/dansbecker/handling-missing-values)