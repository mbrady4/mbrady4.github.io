+++
title = "Set Pandas DataFrame View Options"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
### Set DataFrame view options

Set max columns, max rows, max width
```
    pd.set_option('display.max_rows', 500)
    pd.set_option('display.max_columns', 500)
    pd.set_option('display.width', 1000)
```
[How do I expand the output display to see more columns?](https://stackoverflow.com/questions/11707586/how-do-i-expand-the-output-display-to-see-more-columns)

### Get a dataframe instead of a series when accessing a single column
```
    # Returns DataFrame Object
    df[['column']]
```