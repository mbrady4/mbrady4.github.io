+++
title = "The Curse of Dimensionality"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
## What it is

The curse of dimensionality refers to the challenges and limitations arise from trying process datasets with a large number of features. A few of the specific challenges include: 

- **Sparcity:** As dimensions are added to datasets, the data points become more spread out. Given enough dimensions this can cause clusters/relationships to diminish as all datapoints become space somewhat evenly.
- **Computational Intensive:** Running computational tasks across a dataset with a high number of dimensions inherently takes longer/uses more compute resources than low number of dimensions (assuming the same number of observations).
- **Gaining an intuition for the space:** High dimensional spaces make it difficult to explore and discover relationships between dimensions. Thus, it increases the importance of intuition and hypothesis testing.

Dimensions are hard to visualize but one simple visualization is to imagine the growth in possible spaces a penny could occupy as you move from a 100yd line, field, 30-floor building. 

## How to deal with it

- Rule of thumb: Have at least 5x more observations than dimensions (features)
- A few basic dimension reduction techniques:
    - [Principal Component Analysis (PCA)](https://www.notion.so/Principal-Component-Analysis-PCA-7a715be1056b4bc8b0ea2dd15c56c80d)
    - Feature selection (filter out irrelevant features)
    - Feature extraction (subset the data to capture the most useful information)

## Additional Resources (Beyond Documentation)

[What is a clear explanation of data sparsity?](https://www.quora.com/What-is-a-clear-explanation-of-data-sparsity)

[Curse of Dimensionality](https://towardsdatascience.com/curse-of-dimensionality-2092410f3d27)