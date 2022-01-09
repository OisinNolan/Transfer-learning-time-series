**Things I know about**
- How to do time series classification with various convolutional neural nets
- How to transfer knowledge from one dataset to another using these models
- How to choose which datasets / models to use as pre-trained models

**Models**
- FCN: basic conv net
- TimeNet: RNN
- ConvTimeNet: 
- Encoder: attention mechanism
- Dilated: causal dilated convolutions, triplet loss


**Compare on**
* Architecture
* Interpretability
* Supervision (/  Loss function?)
* Accuracy?
* Complexity?

**Layout**
- Intro
	- Motivation
		- Why TSC?
			- Time series encompasses a broad range of problems: 
			- [[langkvist_2014.pdf]] says most real-world data has a temporal component.
			- Examples include EHRs [[gupta_2021.pdf]], acoustic signals [[cite needed]], and ... [something]
		- Why deep learning models?
			- Traditional methods involve ...
			- DL recently show promise for TSC [[fawaz_2019.pdf]]
			- generally more efficient than other methods (COTE)
			- ability to transfer + fine-tune
		- Why TL?
			- (high-level definition)
			- Huge success in NLP (bert, GPT), CV
			- Sparse label situations (common)
			- Tuning to individuals e.g. patients, devices etc.
	- Research questions
		- To what degree, and under what conditions, is knowledge transferable between time series datasets?
		- What models currently exist that enable us to do this?
		- How should a data scientist decide which one to choose based on the task at hand?
	- What's to come
- Background
	- What is TSC
		- Traditional approaches, and why we look at DL + TL?
	- What is Transfer learning
	- (The various model components discussed?) i.e. the TL model taxonomy
	- UCR dataset?
- Literature Review
	- For each paper:
		- Model description
		- Transfer experiment description
		- (compare + contrast both experiment and model between papers)
	- Aggregation of results
		- Table of model details
		- Accuracy of model from scratch for each architecture
		- Average improvement from trasnferring? But this is very hard to compare as they never have the same source datasets.
- Source selection
	- IDS
	- MSC
- Conclusion
	- What have we discovered?
	- What _obvious_ work remains to be done?
	- What new avenues could be explored in future?


**New ideas**
- Investigate the effect of transferring from the same _type_ of source dataset, e.g. EEG -> EEG. (is there evidence of similar from TL in other domains?)
- Highlight that [[meiseles]] method is only really useful when you have pre-trained models, not if you're training the source model yourself.
- Call for some experiments to be done on transferability across these models where the _source is the same_. All these experiments use different sources, making it hard to compare.
- Look at how fordA as a source dataset interacts with the target datasets of [[franceschi_2020]] according to [[fawaz_2018]].
- Investigate scaling of performance increase relative to amount of (unsupervised) training data.
- It seems that any time they train on large number of datasets there's net positive transfer: the negative transfer comes when it's small source dataset.

