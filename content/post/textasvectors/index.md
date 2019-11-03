+++
title = "Representing text as vectors"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
## Vector Representations

Frequency-based word embedding (bag-of-word) approaches vectorize tokenized documents. This means that each document is a row in the dataframe and a column is created for each unique word in the corpus (group of documents). 

The presence (or absence) of a given word in a document is represented either as a raw count of how many times the word appears in a document (CountVectorizer) or as that word's TF-IDF score (TfidfVectorizer). 

## Generators

A generator object allows a file to be analyzed without reading the entire document into memory. In python, working with generators often involves the `yield` statement:

    import os 
    
    def gather_data(filefolder):
        files = os.listdir(filefolder)
        
        for article in files:
            with open(os.path.join(filefolder, article), 'rb') as f:
                yield f.read()
    
    data = gather_data('./data')
    
    # data is of type generator
    for d in data: 
    	print(d)

## D**ocument Term Matrices (DTM)**

Each column represents a word. Each row represents a document. 

The value in each cell is typically a count of appearance of words but can be other values (e.g., does the word appear at all). 

### CountVectorizer

Creates a document term matrix in which the value in each cell represents the count of the appearance of that word in the document each row represents. 

    from sklearn.feature_extraction.text import CountVectorizer
    
    # create the transformer
    vectorizer = CountVectorizer(stop_words='english', tokenizer=spacy_tokenizer)
    
    # tokenize and build vocab
    vectorizer.fit(data)
    
    # The vocabulary dictionary does not represent the counts of words!!
    dtm = vectorizer.transform(data)
    
    # To DataFrame
    dtm = pd.DataFrame(dtm.todense(), columns = vectorizer.get_feature_names())

    # The vocabulary establishes all of the possible words that we might use.
    vectorizer.vocabulary_

### Term Frequency - Inverse Document Frequency

The purpose of TF-IDF is to find what is unique to each document. Thus, words that are common across all documents are penalized—allowing for unique words (topics) to rise to the top. 

Term frequency is the ratio of a specific word to the total number of words in a document

`(term frequency in document / total words in document)`

Inverse Document Frequency penalizes word that exist in a large number of documents. 

![](https://mungingdata.files.wordpress.com/2017/11/equation.png?w=430&h=336)

    from sklearn.feature_extraction.text import TfidfVectorizer
    
    # Instantiate vectorizer object
    tfidf = TfidfVectorizer(stop_words='english', max_features=5000)
    
    # Create a vocabulary and get word counts per document
    dtm = tfidf.fit_transform(data)
    #dtm = tfidf.transform(new_data)
    
    # View Feature Matrix as DataFrame
    docs = pd.DataFrame(dtm.todense(), columns = tfidf.get_feature_names())
    docs.head()

## Cosine Similarity

Cosine similarity accepts a document term matrix and identifies how similar each document is to every other document. Cosine similarity is can be computationally expensive as it computes the distance between every point and each point.

    from sklearn.metrics.pairwise import cosine_similarity
    
    # Calculate Distance of TF-IDF Vectors
    dist_matrix = cosine_similarity(dtm)
    
    # Turn it into a DataFrame
    dist_df = pd.DataFrame(dist_matrix)

Cosine similarity converted to a DataFrame allows one to easily see which documents a specific document is most similar to:

    dist_df[392].sort_values(ascending=False)[:5]

## NearestNeighbor

A number of approaches use a approach simliar to clustering to reduce the required number of distance calculations by efficiently encoding aggregate distance information. A popular approach is to use the ball tree data structure. The ball tree structure allows a single distance calculation between a test point and centroid to determine a lower and upper bound on the distance to all points within the node. 

    from sklearn.neighbors import NearestNeighbors
    
    nn = NearestNeighbors(n_neighbors=5, algorithm='ball_tree')
    
    # Fit on TF-IDF Vectors
    nn.fit(dtm.todense())

    # Query Using kneighbors 
    nn.kneighbors(dtm.todense()[0])

    # Compare to a out of train document
    new = tfidf.transform(medium_random_ass_tech)
    
    nn.kneighbors(new.todense())

# Word Embedding Models

A key limitation of bag-of-words approaches is that they lose all context around the word. Embedding approaches seek to preserve more textual context. 

Word embedding models are based on the Distribution Hypothesis Theory which posits 'that words that have similar contexts will have similar meanings.'

> Practically speaking, this means that if two words are found to have similar words both to the right and to the left of them throughout the corpora then those words have the same context and are assumed to have the same meaning.

Thus, a very simple word embedding model may generate the following row vectorization: 

[]()

Where each column represents the word before and after the word we are seeking to understand. 

## Skip-gram

The Skip-Gram method predicts the neighbors’ of a word given a center word. In the skip-gram model, we take a center word and a window of context (neighbors) words to train the model and then predict context words out to some window size for each center word.

## Continuous Bag of Words

Continuous bag of words takes the inverse approach of skip-gram. Seeking to predict the center word and expecting the context words as input. 

### Word2Vec

A high-performing pre-trained word embedding model. Word2Vec has a 'window of context' greater than just the word to the left and right. 'Window of context' refers to how far to the left and right a model looks. 

SpaCy has a pre-trained Word2Vec model: 

    # Process a text
    doc = nlp("Two bananas in pyjamas")
    
    # Get the vector for the token "bananas"
    bananas_vector = doc.vector
    print(bananas_vector)

From John Cody: 

    # import the PCA module from sklearn
    from sklearn.decomposition import PCA
    
    def get_word_vectors(words):
        # converts a list of words into their word vectors
        return [nlp(word).vector for word in words]
    
    words = ['car', 'truck', 'suv', 'elves', 'dragon', 'sword', 'king', 'queen', 'prince', 'horse', 'fish' , 'lion']
    
    # intialise pca model and tell it to project data down onto 2 dimensions
    pca = PCA(n_components=2)
    
    # fit the pca model to our 300D data, this will work out which is the best 
    # way to project the data down that will best maintain the relative distances 
    # between data points. It will store these intructioons on how to transform the data.
    pca.fit(get_word_vectors(words))
    
    # Tell our (fitted) pca model to transform our 300D data down onto 2D using the 
    # instructions it learnt during the fit phase.
    word_vecs_2d = pca.transform(get_word_vectors(words))
    
    # let's look at our new 2D word vectors
    word_vecs_2d
    
    # create a nice big plot 
    plt.figure(figsize=(20,15))
    
    # plot the scatter plot of where the words will be
    plt.scatter(word_vecs_2d[:,0], word_vecs_2d[:,1])
    
    # for each word and coordinate pair: draw the text on the plot
    for word, coord in zip(words, word_vecs_2d):
        x, y = coord
        plt.text(x, y, word, size= 15)
    
    # show the plot
    plt.show()

## Training a Word2vec model with tokenized content

    from gensim.models.word2vec import Word2Vec
    
    # text should be tokenized in list format
    model = Word2Vec(TEXT, min_count=1, size=5)
    
    #Find info out about the model
    print(model)
    print(list(model.wv.vocab))
    print(len(model.wv.vocab))
    
    #Get list of words that are similiar to provided word
    model.wv.most_similar('Machine')