# Markov Models in Financial ML
Markov Models attempt to explain a random process that depends on the current event but not on the previous events. It's a special case of a probabilistic/stochastic model.The Markov Chain Model is for a system whose states are fully observable. The Hidden Markov Model (HMM) is for a system whose states are only partially observable. Markov Models output the expected probability of some event or events given the current state <sup>[1](#fn1)</sup>.

Notice that Markov models deal with sequences of inputs - they are for time-series data. A Deep Learning architecture that is also meant to deal with time-series data is the RNN. Markov Models are simpler but also make some broad assumptions that often are not true. When the assumptions are true, however, you may get better results using a HMM than by using a RNN. If you have a large amount of data, though, the RNN will likely outperform the HMM even if the assumptions for an HMM are true <sup>[2](#fn2)</sup>.

# Under Construction
When the current data set I'm iterating on calls for time-series data I will continue this post.

# References
<a name="fn1">[1]</a>: https://blog.quantinsti.com/markov-model/ 

<a name="fn2">[2]</a>: https://stats.stackexchange.com/a/366430
