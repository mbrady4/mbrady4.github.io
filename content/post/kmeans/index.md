+++
title = "K-means clustering"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
## What it is

Clustering is an approach that aims to divide observations into *k* clusters (groups). One of the most basic jobs of the user is to determine the number of clusters the algorithm will create. 

As clustering does not rely upon labels, it is an unsupervised machine learning approach. 

## How it works

1. Select *k* random points to act as the initial centroids (one point per cluster)
2. Assign points to *k* clusters by associating each observation with the nearest centroid
3. Update the location of each centroid by finding the mean 'location' of each cluster

Repeat steps 2-3 until convergence is achieved (essentially the centroids do not change between iterations). 

![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/K-means_convergence.gif/440px-K-means_convergence.gif)

Source: [WikiMedia](https://commons.wikimedia.org/wiki/File:K-means_convergence.gif)

## When to use it

Clustering can be thought of as a 'unsupervised classification' approach. This is because clustering is an effective method to summarize data based on how similar/dissimilar our data is. 

Clustering approaches are unlikely to be reliable enough to be deployed in production. 

## Python Implementation

Assumes that inputted DataFrame contains only relevant features (no labels, no ids, etc.). To determine an appropriate number of clusters, it may be useful to refer to the [elbow method](https://pythonprogramminglanguage.com/kmeans-elbow-method/). 

    # Dependencies
    import pandas as pd
    from sklearn.cluster import KMeans
    
    # Intialize a KMeans object with a target number of clusters
    km = KMeans(n_clusters=4)
    print(km)
    
    # Fit KMeans object to data attributes
    km = km.fit(components)
    print(km)
    
    # Returns array with cluster-distanced spaces for each observation
    result = km.transform(components)
    
    # Returns a series with the cluster each observation belongs to
    labels = pd.DataFrame(km.labels_)
    labels = labels.rename(columns={0:'clusters'})
    
    # If applicable, check the accuracy of the clustering by forming a labeled dataframe
    
    # Create a new labeled To check the effectiveness of the clustering algorithm 
    final = pd.concat([final, labels, target], axis=1)
    
    # Create a crosstab to summarize the effectiveness of the clustering
    pd.crosstab(final['clusters'],final['diagnosis'], margins='index')

## Helpful resources (beyond documentation)

[K-means Clustering in Python](http://benalexkeen.com/k-means-clustering-in-python/)

[](http://localhost:8888/notebooks/Documents/GitHub/DS-Unit-2-Sprint-1-Linear-Algebra/module4-clustering/Cell_Clustering_Assignment.ipynb)