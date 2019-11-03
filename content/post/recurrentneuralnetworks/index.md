+++
title = "Recurrent Neural Networks"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
Using past measurements to predict the future is the foundation of time series analysis. However, any individual past measurement will have some amount of random variation. Thus, most time series analyses should use a smoothing approach: 

- **Moving average:** A rolling average helps to smooth out jumps.
- **Exponential smoothing:** using exponentially decreasing past weights to predict the future

Recurrence relation is an equation that uses recursion to define a sequence (e.g., fibonacci numbers). According to Karpathy,

> The core reason that recurrent nets are more exciting than vanilla neural networks is that they allow us to operate over sequences of vectors; Sequences in the input, the output, or in the most general case both.

![](https://karpathy.github.io/assets/rnn/diags.jpeg)

Recurrent neural networks work with data that may not obviously be a sequence. For example, images can easily be interpreted as sequences. 

Long Short-Term Memory: Able to put more weight on recent events while not completely losing older information. 

On tuning RNNs:

> The two important quantities to keep track of are: (1) The number of parameters in your model. (2)The size of your dataset. 1MB file is approximately 1 million characters.

These two parameters should be key in the same order of magnitude. An example from Karpathy: "I have a 100MB dataset and I'm using the default parameter settings (which currently print 150K parameters). My data size is significantly larger (100 mil >> 0.15 mil), so I expect to heavily underfit. I am thinking I can comfortably afford to make rnn_size larger."

[The Unreasonable Effectiveness of Recurrent Neural Networks](https://karpathy.github.io/2015/05/21/rnn-effectiveness/)