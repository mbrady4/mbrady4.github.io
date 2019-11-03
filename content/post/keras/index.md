+++
title = "Keras"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
> Keras is a high-level neural networks API, written in Python and capable of running on top of TensorFlow, CNTK, or Theano. It was developed with a focus on enabling fast experimentation.

`from keras.models import Sequential` — Means that the model's architecture will be specified one layer at a time

`from keras.layers import Dense` — Means densely connected layer (each node is connected to all layers in previous and next layers)

### Defining a Model

    from keras.models import Sequential
    from keras.layers import Dense
    
    # Initialize a model object
    model = sequential()
    
    # first hidden layer
    # first argument is how many nodes we want in that layer
    # input_dim specifies that an 8 component vector will 
    # be coming into node (same number of features)
    
    model.add(Dense(1, input_dim=8, activation="sigmoid")
    
    # Optimizer is a form of gradient descent
    model.compile(loss='binary_crossentropy', 
                  optimizer='adam', 
                  metrics=['accuracy'])
    
    # Default batch_size is none
    model.fit(X, y, epochs=150, batch_size=32)
    
    # Evaluate the model
    scores = model.evaluate(X, y)
    print(f"{model.metric_names[1]}: {scores[1]*100}")

### A Full Model With MNIST Data

    from tensorflow import keras
    from tensorflow.keras.datasets import mnist
    from tensorflow.keras.models import Sequential
    from tensorflow.keras.layers import Dense
    
    import numpy as np
    
    batch_size = 64
    num_class = 10
    epochs = 50
    
    (X_train, y_train), (X_test, y_test) = mnist.load_data()
    
    # Reshape arrays into 2 dimensions
    X_train = X_train.reshape(60000, 784)
    X_test = X_test.reshape(10000, 784)
    
    X_train = X_train.astype('float32')
    X_test = X_test.astype('float32')
    
    y_train = keras.utils.to_categorical(y_train, num_class)
    y_test = keras.utils.to_categorical(y_test, num_class)
    
    mnist_model = Sequential()
    
    # Hidden
    mnist_model.add(Dense(16, input_dim=784, activation='relu'))
    mnist_model.add(Dense(16, activation='relu'))
    
    # Output Layer
    mnist_model.add(Dense(10, activation='softmax'))
    
    mnist_model.compile(loss='categorical_crossentropy',
                        optimizer='adam', 
                        metrics=['accuracy'])
    
    history = mnist_model.fit(X_train, y_train, batch_size=32, epochs=epochs, validation_split=0.1, verbose=0)
    scores = mnist_model.evaluate(X_test, y_test)
    
    y_pred = mnist_model.predict_classes(X_test)

### Cross Validation

`from sklearn.model_selection import StratifiedKFold` — for cross validation with classification problems

`from sklearn.model_selection import KFold` — for cross validation with regression problems