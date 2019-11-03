+++
title = "Model interpretation & explainability"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
## Explain all the features in relation to one another

1. **Feature Importances:** When interpreting features it is critical to consider that if two or more features are correlated then from the point of view of the model, any of these features can be used as the predictor. Thus it can be somewhat random which feature is selected. 

    n = len(X_train.columns)
    figsize = (5,15)
    
    importances = pd.Series(model.feature_importances_, X_train.columns)
    top_n = importances.sort_values()
    plt.figure(figsize=figsize)
    top_n.plot.barh(color='gray');

**2. Drop-Column Importance:** The best method in theory but too computationally expensive to be practical. Run the model with and without a feature, observe the change in the scoring metric.

    from sklearn.model_selection import cross_val_score
    
    X_train_no_subgrade = X_train.drop(columns='sub_grade')
    new_model = XGBClassifier(max_depth=2, n_estimators=200, n_jobs=-1, random_state=42)
    
    score_with = cross_val_score(new_model, X_train, y_train, cv=2, scoring='roc_auc').mean()
    print('Cross-Validation ROC AUC with sub_grade:', score_with)
    
    score_without = cross_val_score(new_model, X_train_no_subgrade, y_train, cv=2, scoring='roc_auc').mean()
    print('Cross-Validation ROC AUC without sub_grade:', score_without)
    
    print('Drop-Column Importance:', score_with - score_without)

**3. Permutation Importance:** Compromise between feature importance and drop column importance. Train a model and use it to make predictions on a train set. Then create noise (by shuffling) values in a single feature and make predictions on the altered train data. Observe difference.

> The importance of a feature is the increase in the prediction error of the model after we permuted the feature’s values, which breaks the relationship between the feature and the true outcome. (Molnar)

*[From the ELI5 documentation:](https://eli5.readthedocs.io/en/latest/blackbox/permutation_importance.html)* To avoid re-training the estimator we can remove a feature only from the test part of the dataset, and compute score without using this feature. It doesn’t work as-is, because estimators expect feature to be present. So instead of removing a feature we can replace it with random noise - feature column is still there, but it no longer contains useful information. This method works if noise is drawn from the same distribution as original feature values (as otherwise estimator may fail). The simplest way to get such noise is to shuffle values for a feature 

    import eli5
    from eli5.sklearn import PermutationImportance
    
    permuter = PermutationImportance(best, scoring='roc_auc', cv='prefit', 
                                     n_iter=2, random_state=42)
    
    # Note, have to pass in a matrices not Dataframes
    permuter.fit(X_test.values, y_test)
    
    feature_names = X_test.columns.tolist()
    eli5.show_weights(permuter, top=None, feature_names=feature_names)

Depending on the goal of the analysis, either feature permutation should be done on either train or test data. Molnar summarizes the decision as follows:

- *Use train data if you want to know much the model relies on each feature for making predictions*
- *Use test data if you want to know how much each feature contributes to the performance of the model on unseen data*

Note, since permutation relies on random shuffling results can vary with each run. An extremely robust process would run the permutation multiple times and average the results (however this is likely unnecessary in most circumstances)

A simple interpretation from Molnar:

> Feature importance is the increase in model error when the feature’s information is destroyed.

### Drop features with zero importance

    # Assumes permuter estimator has been fit with eli5 library
    mask = permuter.feature_importances_ > 0
    features = X_train.columns[mask]
    X_train = X_train[features]

## Explain individual features relation to the target

### Partial Dependence Plots

> The partial dependence function at a particular feature value represents the average prediction if we force all data points to assume that feature value. (Molnar)

As explained by Christoph Molnar, PDP shows how a feature affects predictions of a model on average. PDP helps to understand if 'the relationship between the target and a feature is linear, monotonous, or more complex.' ([Molnar](https://christophm.github.io/interpretable-ml-book/pdp.html)) To do so, the following process is followed: 

1. Define grid along the feature (e.g., range of values)
2. Model predictions at grid points
3. Line per data instance
4. Average curves to get a PDP 

**Create a partial dependence plot for a single feature**

    from pdpbox.pdp import pdp_isolate, pdp_plot
    
    feature = 'sub_grade'
    
    isolated = pdp_isolate(
        model=best,
        dataset=X_test, 
        model_features=X_test.columns,
        feature=feature
    )
    
    pdp_plot(isolated, feature_name=feature);

**Create a grid to visualize PDP interaction between two features with relation to prediction**

    from pdpbox.pdp import pdp_interact, pdp_interact_plot
    
    features = ['sub_grade', 'dti']
    
    interaction = pdp_interact(
        model=best,
        dataset=X_test,
        model_features=X_test.columns,
        features=features
    )
    
    pdp_interact_plot(interaction, plot_type='grid', feature_names=features);

Note, ideally a histogram showing the distribution of feature values should be presented with a PDP. This shows if the data is sparse in places throughout the distribution—which would indicate that the PDP may be less accurate in those areas.

The main disadvantage of PDP is that it assumes all features are independent. Moreover, when this assumption is not true (often the case) the PDP can end up visualizing 'impossible' data points (e.g., 90 degrees and 6 inches of snow). 

[5.1 Partial Dependence Plot (PDP) | Interpretable Machine Learning](https://christophm.github.io/interpretable-ml-book/pdp.html)

## Explaining Individual Predictions

Use SHAP values. SHAP values break down a prediction to show the impact of a feature.

Per Molnar a shapley value is **"the average contribution of a feature value across all possible coalitions."** The interpretation of a shapley value for a feature is "the value of the feature contributed __ to the prediction of this particular instance compared to the average prediction for the dataset."

> The Shapley value is NOT the difference in prediction when we would remove the feature from the model.

Remember to consider that there may be interaction effects and collinearity between features which is why the Shapley value is not the difference in prediction we would recieve from removing the feature.

Two more interpretation (I find repetition is helpful in this situation): 

1. "The interpretation of the Shapley value is: Given the current set of feature values, the contribution of a feature value to the difference between the actual prediction and the mean prediction is the estimated Shapley value."
2. The Shapley value of a feature value is the average change in the prediction that the coalition already in the room receives when the feature value joins them.

Note, Python's Shap library turns the calculation of shapley values into an optimize problem which results in many of the values to be estimated at zero—this differs from the classic way to derive Shapley values.

    import shap
    shap.initjs()
    
    fn = preds[(y_pred==0) & (y_test==1)]
    fn.sample(n=1)
    
    data_for_prediction = X_test[X_test.index==10]
    
    shap_values = explainer.shap_values(data_for_prediction)
    shap.force_plot(explainer.expected_value, shap_values, data_for_prediction)

[5.8 Shapley Values | Interpretable Machine Learning](https://christophm.github.io/interpretable-ml-book/shapley.html)