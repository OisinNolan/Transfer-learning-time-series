**Title**: Transfer Learning for Time Series Classification: A Review

# Literature Review
## Model Architectures
#### Fully Convolutional Network (FCN)
**Model**
- Proposed by [[wang_2017]], DNN baseline, relatively small (check this) and quick to train
- Just convolutions, batch norm, relus, GAP, and softmax.
- Uses filters of size 3, 5, 8
- CAM for interpretability
- They find that the model performs similarly to COTE

**Transfer Learning**
- TL experiments done by [[fawaz_2018]], who tried transferring from each UCR to each other UCR.
- Fine tune: swap out softmax to match classes, tune encoder+mapping.
- Evidence that any of the datasets can _either_ improve or disimprove from transfer, depending on the source. But, with smart source selection we can get it to improve 71/85% of the time.

#### Encoder
**Model**
- Like [[wang_2017]], 3 conv blocks + aggregation on the time dimension. Unlike [[wang_2017]], this model use max-pooling between conv blocks, instance norm rather than batch norm, parametrizes the relu, and adds dropout for regularization. It aggregates on the time series using an attention mechanism rather than GAP.
- Use larger kernel sizes than [[wang_2017]]: 5, 11, 21, which they found improved accuracy.
- Serra found that these changes to the network gave them better results than wang's architecture.

**Transfer Learning**
- Train on data from 6 types, train on the other. They find that the model performs similarly to COTE, while being more efficient.
- They adopt multi-headed training regime for encoder, and try fine tuning just a classifier and also the entire network per task. They get a 3% relative improvement on average by tuning whole network instead of SVM classifier.
- They find that the Encoder-ADAPT performs better than a from-scratch model, providing some evidence for cross-domain transferability. Further evidence is that the encoders that werent fine tuned at all still did pretty well.
- They find that sometimes transfer is positive, sometimes negative, in accordance with [[fawaz_2018]].

#### ConvTimeNet
**Model**
- Slightly more complex than the previous models, most similar to [[wang_2017]] in that it uses GAP, batch norm, and standard relus.
- What it _adds_ is one extra conv block (depth) and residual connections.
- Also use more filters (5 instead of 3) and of much bigger (exp.) sizes: 4, 8, 16, 32, 64, enabling CTN to capture features along much larger time scales. They show that having varied length filters significantly improves performance.
- So it's basically a more powerful version of [[wang_2017]].

**Transfer Learning**
- Multi head training on all 85 UCR sets, then fine-tune entire model for transfer.
- Transfer beats or draws with 'from scratch' model on vast majority, although improvement isn't huge overall, just 4% relative.
- The fact that the encoder was trained on the sets that it would later be fine-tuned on means that it has actually already seen this data before and so less 'transfer' is occuring, in a sense. More transfer happens in [[serra_2019]] or [[fawaz_2018]] who fine tune the encoder on unseen data.
- (Could talk about interpretability here too)

#### TimeNet
**Model**
- RNN with GRUs, dropout applied to non-recurrent connections, 
- Encoder architecture represents time series in a fixed length hidden vector, $h_T$, the final hidden state of the RNN, containing information from all T steps in the input.
- Multiple recurrent layers are stacked to create depth in the network, enabling hidden layers to learn features at multiple levels of abstraction.
- The decoder is given $h_T$ and procudes a reconstruction of the input sequence (in reverse).
- The decoder architecture is same as input but with a linear layer to map from hidden state to output.
- The encoder thus serves the same purpose as the CNNs discussed so far: encode variable length input sequences as a fixed-length vector
- Self-supervised learning schema based on reconstruction error as loss function.

**Transfer Learning**
- Train encoder on 18 datasets from UCR chosen to be length T<512. Validated on another 6.
- Evaluation on 30 datasets, using train / test split to train SVM with RBF kernel on encoded representation.
- We see a 10% improvement on average relative to SAE. This is significant, and provides evidence that unlabelled data can help improve performance on supervised tasks.
- The s.t.d. was 22% though, showing further evidence that both positive and negative transfer can occur, again motivating smart source selection.

#### Causal-CNN
**Model**
- Exponentially dilated causal convolutions, applying them for the first time to TS representation learning.
- Allow GPU parallelization -> more efficient than RNNs of [[malhotra_2017]].
- Exponential increase of receptive field captures long-range dependencies in the TS.
- Causal convolutions map s1..t -> v1..t using only s1..j to calculate vj
- Consists of convolutional blocks like the other CNNs, uses leaky ReLU activation, weight norm for normalization, and 1x1 convolutions as residual connections, like [[kashiperekh_2019]], to facilitate depth. This model set depth to 10 convolutional blocks, making this encoder much deeper than those discussed previously.
- Finally, global max pooling layer used to aggregate on time dimension. + linear transformation to map to class probabilities.
- Encoder trained using triplet loss: only need encoder, unlike [[malhotra_2017]].
- (summarize triplet loss)

**Transfer Learning**
- Main experiments train encoder unsupervisedly and fit a classifier to the representations on the same set, showing that it achieves acuracy comparable to the best supervised network, HIVE-COTE.
- They include a demonstration that the model learns transferable features by also training the encoder on FordA and fitting SVM to other datasets. It shows positive transfer on some, and negative on others, but never significantly degrades the performance, showing that the encoder is capable of learning universally applicable time series features.
- Interestingly, the positive and negative transfer caused by FordA as source in this paper does _not match_ that provided by [[fawaz_2018]], in some cases having the opposite effect: e.g. with FordA source, [[fawaz_2018]] finds a significant positive transfer, but [[franceschi_2020]] shows a negative transfer. This provides evidence that the effect of source selection depends on the transfer schema being applied: [[fawaz_2018]] _also_ tunes the encoder when transferring!

#### TS2Vec
**Model**
- Very recent SOTA for unsupervised TS representation learning.
- Has a similar structure to [[franceschi_2020]], using dilated receptive field, but _not_ causal convolutions. Same depth (number of convolutions).
- Other differences are that it uses GELU (which regularizes + activations) instead of leaky relus.
- And of course different loss function, see [[yue_2021]]

**Transfer Learning**
- See [[yue_2021]]. It's basically the same experiment as [[franceschi_2020]], although seems to do better.

## Source Selection
**Why?**
- Metrics that help choose source without explicitly training + evaluating each possible model, which is likely computationally infeasible.
- Model-agnostic methods of determining source -> can be applied to future better models
- (also check [[weber_2021]] for this)

#### Inter-dataset Similarity
- [[fawaz_2018]] use DTW distance to compute similarities between datasets
- Calculate a _prototype_ time series per class using DBA averaging method
- The similarity between datasets X and Y is the minimum DTW between class prototypes.
- Using this method, they achieved positive transfer in 71/85 cases, rather than approximately 50/50 pos neg transfer.

#### Mean Silhouette Coefficient
- [[meiseles_2020]] propose a method for when you already have pre-trained models, as opposed to [[fawaz_2018]] which helps decide which data to train on.
- The method directly compares the clustering between classes in representation space created by each model, using MSC.
- MSC compares how close vectors of the same class are to how close vectors of the next nearest class are.