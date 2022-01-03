**summary**
- They propose a method for choosing source model given only a set of source models, not the data they were trained on.
- Use FCN [[wang_2017]].
- They use _Mean Silhouette Coefficient_ metric to measure quality of clusters produced by encoder. This is the mean of $s(i)$ for all $i$ in the encodings of the dataset. This too gives a value between -1 and 1 indicating the goodness of cluster definition.
- They prove that MSC=1 results in perfect classification, and thus hypothesise that higher MSC gets better classification.
- So they use MSC over the target dataset encodings to rank encoders.

**Thoughts**
- This provides an encoder model agnostic way to choose which encoder to use that should be more efficient than fitting each of the softmax layers and computing loss. That's pretty cool.
- This method finds the best model more often than [[fawaz_2018]].
- An obvious drawback is that this method requires you to have the trained models, so it's _kinda_ unfair to compare to [[fawaz_2018]] without mentioning that. The purpose of fawaz method is to save you from having to train all those models. This method is thus better for if you're using pre-trained models, but fawaz is probably better if you're training the source model yourself and need to choose a source task (or multiple).

**Silhouette Coefficient**
- For silhouette coeffecient $s(i)$ for a given vector $i$ to be high, $a$ needs to be small and $b$ big, relative to $a$.
- So we want $a$ to be small, i.e. the vector $i$ should be near the other vectors in class $A$.
- We also want $b$ to be large. $b$ gives the mean distance of $i$ to the vectors from $C$, _the nearest other class_.
- So $s(i)$ returns a number between -1 and 1 where 1 means the $A$ cluster is well defined and -1 means $A$ is poorly defined.
- $s(i) < 0 \iff b < a$, when $i$ is closer to $C$ vectors than its own class.