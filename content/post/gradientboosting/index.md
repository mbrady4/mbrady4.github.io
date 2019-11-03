+++
title = "Gradient Boosting"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Gradient boosting involves three elements:

1. A loss function to be optimized.
2. A weak learner to make predictions.
3. An additive model to add weak learners to minimize the loss function.

### Additive Modeling

Adding a bunch of subfunctions to create a composite function that models some data points. 

Common constraints to place on decision trees to ensure they do not become 'too strong' and quickly overfit the test data: 

- **Number of trees**, generally adding more trees to the model can be very slow to overfit. The advice is to keep adding trees until no further improvement is observed.
- **Tree depth**, deeper trees are more complex trees and shorter trees are preferred. Generally, better results are seen with 4-8 levels.
- **Number of nodes or number of leaves**, like depth, this can constrain the size of the tree, but is not constrained to a symmetrical structure if other constraints are used.
- **Number of observations per split** imposes a minimum constraint on the amount of training data at a training node before a split can be considered
- **Minimim improvement to loss** is a constraint on the improvement of any split added to a tree.

### Early Stopping

Early stopping means training until the validation error does not decrease for a specified number of iterations. In the case of the GBM, this means training more decision trees, and in this example, we will use early stopping with 100 rounds, meaning that the training will continue until validation error has not decreased for 100 rounds. Then, the number of estimators that yielded the best score on the validation data will be chosen as the number of estimators to use in the final model.