**summary**
* Propose ConvTimeNet = CTN
* The ideal CNN filter size will likely vary as time series length varies. Longer filters will capture longer trends
* CTN uses multiple 1-D convolutional filters of varying length to capture features at multiple time scales
* COTE is accurate but has poor time complexity as it is an ensemble method
* transfer learning as a method of _regularization_
* CTN-T (pre-trained CTN) adapts faster to new data than model trained from scratch -> superior for low-data situations.


**model traits**
* Just defined for univariate time series
* Looks at features of up to length 64 initially, which are then combined in later layers.
* Use of GAP (aggregate on time dim.) layer as final layer to reduce param count
* Creates a fixed-length vector representation of input TS and connects that to $S$ softmax layers with $S$ different linear transformations, where the model is trained on $S$ datasets simultaneously (transfer learning).
* So the CTN params are optimised via $S$ datasets that it trains on all at the same time.
* The task-specific weights are tuned per task
* In order to transfer: initialise CTN weights to those learned during training, and final linear transformation initialise randomly, then train the whole thing as part of 'fine tuning' to the new task.
* Fig 7. shows that filters learn some basic time series patterns

**advantages**
* Can cater to diverse length time series (somewhat skeptical of this tbh)
* Performs better than ResNet per training dataset size (due to transfer)


**results**
- 