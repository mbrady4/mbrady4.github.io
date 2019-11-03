+++
title = "Interpretting Correlations"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++

Rule of thumb for interpreting correlation: 

- 0 -0.19 —> 'very weak'
- 0.2-0.4 —> 'weak'
- 0.4-0.59 —> 'moderate'
- 0.6-0.79 —> 'strong'
- 0.8-1.0 —> 'very strong'

### Create sorted list of correlations with target
```
    # Find correlations with the target and sort (ascending)
    coors = df.corr()['TARGET'].sort_values()
```