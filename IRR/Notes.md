#### What to write about?
Want to categorize methods in transfer learning for time series data. I can find papers that do this, either explicitly or by presenting new methods in representation learning for time series (which can then be used for transfer). I can compare these representations on a number of traits, e.g.

* interpretable
* univariate / multivariate
* model qualities? i.e. neural network based, deterministic etc.

Can also try to compare their performance if there are common datasets.

Can also try to taxonomize the methods

* Loss functions
* Evaluation methods / datasets
* Advantages / disadvantages (for specific domains?)

Fawaz 2019 just focuses on end-to-end discriminative models (deep supervised). We could consider some of the more recent (i.e. best) models, and compare to simple baseline (DTW 1NN) across as many axes as we think the reader might care about in choosing a model. We can use the style / taxonomy of Fawaz but with more recent models.

* how does similarity in input distribution of pre-training datasets and target dataset distribution affect performance?

* What are the meaningful differences between FCN, ConvTimeNet, and Encoder?

#### Research Questions
* Comparing time series representations for classification. What ways can we represent time series data so that it can be classified easily? What do the methods compare in terms of accuracy on UCR, efficiency, interpretability?
* Something like [langkvist_2014](obsidian://open?vault=IRR&file=papers%2Fsurveys%2Flangkvist_2014.pdf) but for both classical and deep-learning methods, considering interpretability, complexity (environmental impact), among other things as a relevent factor. Basically evaluating TSC methods, both old and new, with a new lens (we care about interpretability + efficiency) which might end up favoring older methods.
* Whether I discuss more specifically the _representations_ generated (focus on transfer learning?) or just the _models_ themselves is yet to be determined.
* How can CNNs be used to learn fixed-length vector representations of time series with varying length? What methods exist for this, and how do they compare in terms of [...]?

#### Questions
* Is representation learning always used for transfer learning? Like am I comparing representations here? Can I just examine the representations used in the trasnfer learning papers (I think so)? If so then my review is a comparison of representation learning methods with emphasis on how these representations can be used, what traits they have
* How do pre-trained models relate to this?
* What is the relationship between: (classification models) (representations) (transfer learning)?

#### Insights
* TL could be quite useful in _personalised_ predictions / classification, where people are generally similar but vary in some small ways. If a small amount of labelled data is generated per-user, then we can fine tune on that a pre-trained model that has been trained on data from all users. This may be equivalent to just doubling the training data for the user in question, or weighting it somehow? (actually this depends on whether we freeze layers during fine-tuning or not!)
* [[kashiperekh_2019]] and [[serra_2019]] both use supervision (on multiple UCR tasks) to train their encoders, whereas [[franceschi_2020]] and [[malhotra_2017]] train encoders _unsupervisedly_, though by different means.
* The reason euclidean distance isn't great for TS is because you could have the same pattern slighlty shifted left or right and it'd be really far away because we compare element-wise for euclidean.