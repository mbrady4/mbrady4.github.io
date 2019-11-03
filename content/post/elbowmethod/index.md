+++
title = "The elbow method: optimizing K-means clustering"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
## What it is

The elbow method assists with selecting the optimal K value (# of centroids) which is a critical input to a K-means algorithm (the # of centroids determines the # of clusters).

## How it works

1. Select a range of potential K values to test (likely start at 1)
2. Determine the sum of squared distance by accessing the .inertia_ attribute of a fitted KMeans object
3. Visualize the sum of squared distances for the range of tested K values
4. Identify the 'elbow' in the visualization (where the magnitude of the negative slope sharply decreases). The K value that corresponds with the 'elbow' is a good starting point to find the optimal K value

## When to use it

When a visual inspection of the data is not possible (more than 2/3 dimensions) or does not provide an obviously optimal # of centroids

## Python Implementation

    # Dependencies
    from sklearn.cluster import KMeans
    
    # List to hold result for each tested K value
    sum_of_sqaured_distances = []
    
    # Range of potential clusters to be tested
    K = range(1,11)
    
    # Loop through each potential K value
    for k in K:
    
        # Initialize KMeans object with k clusters
        km = KMeans(n_clusters=k)
    
        # Fit the KMeans object to the input data
        km = km.fit(df)
    
        # Append intertia of fitted KMeans object to list
        # Intertia is the within-cluster sum of sqaured distances from centroid
        sum_of_sqaured_distances.append(km.inertia_)
    
    # Visualizing results to identify elbow
    plt.plot(K, sum_of_sqaured_distances, 'bx-')
    plt.xlabel('k')
    plt.ylabel('Sum_of_sqaured_distances')
    plt.show()

## Additional Resources (Beyond Documentation)

[Tutorial: How to determine the optimal number of clusters for k-means clustering](https://blog.cambridgespark.com/how-to-determine-the-optimal-number-of-clusters-for-k-means-clustering-14f27070048f)