**summary**
* Builds an encoder that produces time series representations
* Evaluates how well it transfers to other datasets
* They focus on having a pre-trained encoder that can transfer to _different types_ of time series data i.e. rainfall measurements -> ECG measurements. Both are time series but different _types_.
* Universal encoders exist in other domains: the idea is to produce generally useful representations that can be used for a variety of tasks with minimal or no adaptation.
* They split UCR up into 7 data types which they've identified, to ensure that transfer can occur across data type.
* They then train the encoder on datasets from 6 of the 7 types, then fine-tune on the 7th ones train set and evaluate on the 7th ones test set.
* They use 1NN to examine representations _without fine tuning_ i.e. how good are the clusters that the encoder generates
* They use LogReg and SVM to perform _some_ fine tuning (i.e. freezing the encoder)
* They also try fine-tuning the entire network (Encoder-ADAPT) and compare that to training from scratch. Fine tuning the encoder + classification head (FC layer) results in best performance, benefitting from transfer.
* Encoder-ADAPT is beaten by COTE, but is probably more efficienct.
* Encoder-ADAPT performs very well on some data sets, but poorly on others. i.e. the performance is variable.

**model traits**
* convolutional attention mechanism -> fixed-length vector representation
* Convolutions to capture + compose local / temporal relationships
* Attention mechanism to convert to fixed-length vector
* They use a multi-head training approach as in ConvTimeNet [[kashiperekh_2019]], with linear + softmax for each dataset to fit the right number of classes.
* They specify in the second last section how they modified Wang's model to produce theirs.

**questions**
* How is the attention activation vector $a$ calculated?