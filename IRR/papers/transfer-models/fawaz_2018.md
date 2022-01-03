**summary**
* They train a model on each task in UCR, and then fine each one on _every other task_ in UCR, producing 7140 models in total.
* This allows them to examine when models improve or degrade in performance given their pre-training set.
* They measure similarity between datasets using DTW to try detemine if transfer will be useful or not.
* Transfer learning as a method of regularization, to help prevent CNNs overfitting in small-dataset scenarios, where they are outperformed in accuracy by 1NN DTW.
* They use FCN [[wang_2017]] (fully convolutional) model.
* They show that for any given target dataset, there is always a source dataset that will disimprove accuracy, keep it the same, or improve it.
* Experimental evidence of _negative transfer learning_.
* If a target dataset is _big enough_, then transferring doesn't help much.
* They find that similar datasets are similar semantically in the case of HandOutliers
* Small target dataset almost always improves for transfer
* They show an exmaple of similarity -> accuracy boost
* They also showed that close DTW neighbours in the case of Meat were _also spectrographs_ giving some evidence that time series datasets of same _type_ might be better for transfer.

**Model traits**
* FCN
* Transfer occurs by pre-training the CNN on one dataset, then swapping out the softmax layer to a randomly initialized one that matches the target dataset class number and fine-tuning the entire model on the target dataset.

**Dataset similarity**
- For each dataset generate a _prototype_ time series per class by averaging the set of time series.
- Distance between two datasets is then the minimum distance between prototypes of their classes.