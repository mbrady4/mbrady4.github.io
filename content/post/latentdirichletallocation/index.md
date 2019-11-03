+++
title = "Latent Dirichlet Allocation (LDA)"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
LDA is an popular topic modeling approach which is used to classify text in a document to a particular topic. 

> The probabilistic topic model estimated by LDA consists of two tables (matrices). 

The first table describes the probability or chance of selecting a particular part when sampling a particular topic (category).

The second table describes the chance of selecting a particular topic when sampling a particular document or composite.

Two critical LDA hyperparameters:

- The *alpha* controls the mixture of topics for any given document. Turn it down, and the documents will likely have less of a mixture of topics. Turn it up, and the documents will likely have more of a mixture of topics.
- The *beta* hyperparameter controls the distribution of words per topic. Turn it down, and the topics will likely have less words. Turn it up, and the topics will likely have more words.

    import gensim
    
    from gensim.utils import smart_open, simple_preprocess
    from gensim.parsing.preprocessing import STOPWORDS
    from gensim import corpora
    
    from gensim.models.ldamulticore import LdaMulticore

Read tokenize files from folder:

    def doc_stream(path):
        for f in os.listdir(path):
            with open(os.path.join(path,f)) as t:
                text = t.read().strip('\n')
                tokens = tokenize(str(text))
                yield tokens

LDA Topic Modeling

    # A Dictionary Representation of all the words in our corpus
    id2word = corpora.Dictionary(doc_stream(path))
    
    # Let's remove extreme values from the dataset
    id2word.filter_extremes(no_below=10, no_above=0.75)
    
    # analogous to CountVectorizer
    corpus = [id2word.doc2bow(text) for text in doc_stream(path)]
    
    lda = LdaMulticore(corpus=corpus,
                       id2word=id2word,
                       random_state=723812,
                       num_topics = 15,
                       passes=10,
                       workers=4
                      )

## Coherence

A technique to determine the optimal number of topics for topic modeling. 

> Each such generated topic consists of words, and the topic coherence is applied to the top N words from the topic. It is defined as the average / median of the pairwise word-similarity scores of the words in the topic (e.g. PMI).

    from gensim.models.coherencemodel import CoherenceModel
    
    def compute_coherence_values(dictionary, corpus, path, limit, start=2, step=3):
        """
        Compute c_v coherence for various number of topics
    
        Parameters:
        ----------
        dictionary : Gensim dictionary
        corpus : Gensim corpus
        path : path to input texts
        limit : Max num of topics
    
        Returns:
        -------
        model_list : List of LDA topic models
        coherence_values : Coherence values corresponding to the LDA model with respective number of topics
        """
        coherence_values = []
        model_list = []
        for num_topics in range(start, limit, step):
            stream = doc_stream(path)
            model = LdaMulticore(corpus=corpus, num_topics=num_topics, id2word=id2word, workers=4)
            model_list.append(model)
            coherencemodel = CoherenceModel(model=model, texts=stream, dictionary=dictionary, coherence='c_v')
            coherence_values.append(coherencemodel.get_coherence())
    
        return model_list, coherence_values