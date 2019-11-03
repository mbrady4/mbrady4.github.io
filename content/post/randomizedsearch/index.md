+++
title = "Hyperparameter tuning with randomized search"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
> In contrast to GridSearchCV, not all parameter values are tried out, but rather a fixed number of parameter settings is sampled from the specified distributions. The number of parameter settings that are tried is given by n_iter.

Using GridSearch and RandomizedSearch together:

1. Use random search with a large hyperparameter grid
2. Use the results of random search to build a focused hyperparameter grid around the best performing hyperparameter values.
3. Run grid search on the reduced hyperparameter grid.
4. Repeat grid search on more focused grids until maximum computational/time budget is exceeded.

[Intro to Model Tuning: Grid and Random Search](https://www.kaggle.com/willkoehrsen/intro-to-model-tuning-grid-and-random-search)