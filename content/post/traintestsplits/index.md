+++
title = "Performing Train-Test Splits"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
### Test / Test Sets:

from sklearn.model_selection import train_test_split
    
    train, test = train_test_split(df, test_size = 0.2, random_state = 42)
    train.shape, test.shape

### Train / Validation / Test Sets**:**

    from sklearn.model_selection import train_test_split
    
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=42, stratify=y)
    
    X_train, X_val, y_train, y_val = train_test_split(
        X_train, y_train, test_size=0.3, random_state=42, stratify=y_train)
    
    X_train.shape, X_val.shape, X_test.shape, y_train.shape, y_val.shape, y_test.shape

### Option 2:

    from sklearn.model_selection import train_test_split
    
    # Partition the dataset in train + test sets
    X_train, X_test, y_train, y_test = train_test_split(df, y, test_size = 0.2, random_state = 42)
    print("X_train : " + str(X_train.shape))
    print("X_test : " + str(X_test.shape))
    print("y_train : " + str(y_train.shape))
    print("y_test : " + str(y_test.shape))

Note, standardization (e.g., StandardScaler) cannot be done before  creating test and train sets as you don't want to fit StandardScaler on observations that will later be used in the test set.