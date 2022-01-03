**Summary**
* Main contributions: applying _causal dilated convolutions_ to TSC, proposing a novel _triplet loss_.
* Motivations: data is rarely labelled, input length varies, scalability w.r.t. length
* This is better than each previous methods in at least one way
* triplet loss has been used in representation learning for other domains (e.g. word2vec)
* CNNs outperform RNNs in both efficiency and accuracy for TSC
* Only train encoder = more efficient
* Showed transferability by using an encoder trained on FordA. Can see if FordA was a good one to train on via [[fawaz_2018]] heatmap.


**Triplet loss**
* Draws inspiration from Word2Vec training
* Let f(x) be the encoder. This loss function incurs _lower loss_ when f(x_ref) is close to f(x_pos), and _higher loss_ when f(x_ref) is close to f(x_neg). Thus, during training, the parameters of f are tuned such that f(x_ref) is close to f(x_pos) and far from f(x_neg).
* The name _triplet loss_ comes from the fact that the loss function takes a triple for each sample in the training data: x_ref, x_pos, and x_neg. (although we take _k_ x_negs to help convergence).
* The complexity is linear in the time of f(x)


**Exponentially dilated causal convolutions**
* Convolutional layers, but:
* _exponentially dilated_ means the size of the receptive field increases exponentially with depth. we start by looking at windows of size $2^0$ in the input, get some hidden representation, then look at windows of size $2^1$ on that, and $2^2$ on the following hidden layer, and so on. This effectively allows us to look back further in time with each new layer than e.g. linearly increasing receptive field size.
* _causal convolutions_ only use input from up until time $t$ to generate output at $t$. An advantage of this is that the model can make online predictions efficiently: it doesn't have to update previously made predictions each time we get some new input appended to our time series.
* Global max pooling aggregates the time series of arbitrary length to produce a fixed length vector representation.


**Models considered**
* HIVE-COTE is an ensemble method consisting of a set of other popular models: ST, BOSS, EE.


**Advantages of this model**
* Much more performant in _sparsely labelled_ time series, i.e. lots of inputs but only a few outputs. This is lots of real world data, probably.
* Representation transforms time series into a useful metric space where l_2 distance of input corresponds with similarity in output, better than DTW.
* Experimental evidence of transferability!
* Super scalable