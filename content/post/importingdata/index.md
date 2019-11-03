+++
title = "Importing Data"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Michael W. Brady"]
+++

**Error:** *Error tokenizing data. C error: EOF inside string starting at line*

**Solution:** Specify parsing engine as python

    ```df = pd.read_csv('tracks.csv', engine='python')```

**Error:** *unexpected end of data*

**Solution:** Tell Pandas to skip bad lines

    ```df = pd.read_csv('tracks.csv', error_bad_lines=False)```

**Error:** *File has unwanted rows prior to header row*

**Solution:** Utilize the skiprows attribute 

    ```df = pd.read_csv('tracks.csv', skiprows=1)```

Sources:

[Pandas CSV error: Error tokenizing data. C error: EOF inside string starting at line](https://www.shanelynn.ie/pandas-csv-error-error-tokenizing-data-c-error-eof-inside-string-starting-at-line/)