+++
titie = "Accessing Column Datatypes"
date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Print a series listing each column and the column's datatype: 

    df.dtypes

Get a count of columns by datatype:

    # Number of each type of column
    app_train.dtypes.value_counts()

[Start Here: A Gentle Introduction](https://www.kaggle.com/willkoehrsen/start-here-a-gentle-introduction)

Select columns by datatype:

    df.select_dtypes(include='number')
    df.select_dtypes(exclude='number')
    
    # Watch out for columns of 'category' type when accessing objects 
    df.select_dtypes(include='object')