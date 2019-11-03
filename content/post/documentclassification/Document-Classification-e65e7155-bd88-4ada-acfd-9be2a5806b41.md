+++
title = "Document Classification"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
## Latent Semantic Analysis (or Indexing)

Latent Semantic Analysis is a technique for creating a vector representation of a document. Having a vector representation of a document gives you a way to compare documents for their similarity by calculating the distance between the vectors. This in turn means you can do handy things like classifying documents to determine which of a set of known topics they most likely belong to. ([McCormick](https://mccormickml.com/2016/03/25/lsa-for-text-classification-tutorial/))

The first step of LSA is to create a DTM with tf-idf. LSA than utilizes SVD to perform dimensionality reduction on the vectors created by tf-idf

An LSA pipeline:

    from sklearn.pipeline import Pipeline
    
    from sklearn.linear_model import SGDClassifier
    from sklearn.feature_extraction.text import TfidfVectorizer
    from sklearn.decomposition import TruncatedSVD
    
    vect = TfidfVectorizer(stop_words='english')
    sgdc = SGDClassifier()
    
    svd = TruncatedSVD(n_components=100, 
                       algorithm='randomized',
                       n_iter=10)
    
    lsi = Pipeline([('vect', vect), ('svd', svd)])
    pipe = Pipeline([('lsi', lsi), ('clf', sgdc)])
    
    pipe.fit(data.data, data.target)

Useful setting to expand column width as needed:

    pd.set_option('display.max_colwidth', 200)

LSA is a technique which attempts to capture the context around words to determine meaning

**Pros:**

- LSA is fast and easy to implement.
- It gives decent results, much better than a plain vector space model.

**Cons:**

- Since it is a linear model, it might not do well on datasets with non-linear dependencies.
- LSA assumes a Gaussian distribution of the terms in the documents, which may not be true for all problems.
- LSA involves SVD, which is computationally intensive and hard to update as new data comes up.

[Text Mining 101: A Stepwise Introduction to Topic Modeling using Latent Semantic Analysis (using Python)](https://www.analyticsvidhya.com/blog/2018/10/stepwise-guide-topic-modeling-latent-semantic-analysis/)

## Example of Feeding a DTM into a SkLearn Estimator

    from sklearn.linear_model import LogisticRegression
    
    LR = LogisticRegression(random_state=42).fit(X_train_vectorized, y_train)
    
    train_predictions = LR.predict(X_train_vectorized)
    test_predictions = LR.predict(X_test_vectorized)
    
    from sklearn.metrics import accuracy_score
    
    print(f'Train Accuracy: {accuracy_score(y_train, train_predictions)}')
    print(f'Test Accuracy: {accuracy_score(y_test, test_predictions)}')
    
    columns = ['model', 'acc_train', 'acc_test', 'vect']

Topic models are unsupervised learning techniques which discover topics across various text documents.