+++
title = "Building Pipelines with Sklearn"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
Sequentially apply a list of transforms and a final estimator. Intermediate steps of the pipeline must be ‘transforms’, that is, they must implement fit and transform methods. The final estimator only needs to implement fit.

## Build a preprocessing pipeline

    import pandas as pd
    import category_encoders as ce
    
    from sklearn.compose import ColumnTransformer, make_column_transformer
    from sklearn.decomposition import PCA
    from sklearn.impute import SimpleImputer
    from sklearn.pipeline import make_pipeline, Pipeline
    from sklearn.preprocessing import StandardScaler
    
    pca_processing = make_pipeline(
        StandardScaler(),
        SimpleImputer(strategy='mean'),
        PCA(n_components=15)
    )
    
    hash_processing = make_pipeline(
        ce.HashingEncoder(n_components=8)
    )
    
    one_hot_processing = make_pipeline(
        ce.OneHotEncoder(use_cat_names=True)
    )
    
    preprocess = make_column_transformer(
        (hash_processing, 'producer'),
        (hash_processing, 'location'),
        (hash_processing, 'composer'),
        (one_hot_processing, 'type'),
        (pca_processing, pca_features)
    )

## Pair with a model execution pipeline

    from sklearn.pipeline import make_pipeline, Pipeline
    from sklearn.linear_model import LogisticRegression
    
    model = make_pipeline(
        preprocess,
        LogisticRegression(solver='lbfgs', max_iter=2000, multi_class='auto')
    )

[A Deep Dive Into Sklearn Pipelines](https://www.kaggle.com/baghern/a-deep-dive-into-sklearn-pipelines)

[Introducing the ColumnTransformer: applying different transformations to different features in a scikit-learn pipeline | Joris Van den Bossche](https://jorisvandenbossche.github.io/blog/2018/05/28/scikit-learn-columntransformer/)

[Building Scikit-Learn Pipelines With Pandas DataFrames](https://ramhiser.com/post/2018-04-16-building-scikit-learn-pipeline-with-pandas-dataframe/)

[Column Transformer with Mixed Types - scikit-learn 0.20.3 documentation](https://scikit-learn.org/stable/auto_examples/compose/plot_column_transformer_mixed_types.html#sphx-glr-auto-examples-compose-plot-column-transformer-mixed-types-py)

## Access steps within a pipeline

Two options shown on [StackOverFlow](https://stackoverflow.com/questions/28822756/getting-model-attributes-from-scikit-learn-pipeline):

    pipeline.named_steps['pca']
    pipeline.steps[1][1]

## Grid Search With Pipelines

    from sklearn.model_selection import GridSearchCV
    
    parameters = {
        'vect__max_df': (0.5, 0.75, 1.0),
        'clf__max_iter':(20, 10, 100)
    }
    
    grid_search = GridSearchCV(pipe,parameters, cv=5, n_jobs=-1, verbose=1)
    
    grid_search.fit(data.data, data.target)