**summary**
* RNN-based feature extractor (representation learner) for time series.
* Timenet = encoder network of an auto-encoder (i.e. it's been trained to _re-construct_ the time series -- this differs from [[kashiperekh_2019]] and [[serra_2019]], which were both trained to perform classification, and [[franceschi_2020]] which was context-based)
* They compare timenet to a from-scratch AE and a DTW NN classifier.
* t-SNE plots of learned representations provide some evidence that timenet captures important time series features.
* Use SVM as classification head like [[serra_2019]].
* They outperform DTW-C on the vast majority of tasks, and most of the time outperform task-specific SAE-C (their from-scratch model) 

**model traits**
* The RNN consists of GRUs with dropout. GRUs are like basic recurrent units but with gates to control what is remembered from the previous hidden layer and what is output to the current hidden layer.
* Model trained as an auto-encoder: loss function is _reconstruction error_. Encoder part then used as feature extractor. The encoder model is thus unsupervised.
* Final hidden layer in the encoder is the representation: it must contain enough information to reconstruct the entire time series, thus representing it.
* Complexity is linear in the length of the time series
* univariate !