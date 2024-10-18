#model

Paper: https://arxiv.org/abs/2211.12054
Code: https://github.com/thunlp/CLEVER
# Main idea

**Commonsense knowledge extraction (CKE)**
	which aims to extract plausible commonsense interactions between entities

CKE is a **Distantly Supervised multi-instance learning problem**
	Contrastive Instance Learning (CIL)

# Model

## CLEVER
![[Pasted image 20241010184652.png#pic_center]]

It formulates CKE as a distantly supervised multi-instance learning problem 
	Models learn to summarize general commonsense relations of an entity pair from a bag of images.

![[Pasted image 20241011145237.png#pic_center|400]]

- What is Distant Supervision (DS) learning?
	It is one method in Semi-supervised learning domain. The main idea is to automatically generate labeled training data through existing knowledge database. 

- What is the problem and possible solutions of DS learning?
	Problem: Noise
	Description: The synthesized label may not absolutely correct.
	Solutions:
		Multi-instance learning (MIL)
			Contrastive Instance Learning (CIL)
		Attention mechanism
### Data process

- What is bag? What is instance? What is the goal of multi-instance learning (MIL)?
	Bag is a container including many instances. Instance is like an image or a sentence.
	Each bag has a label, which indicates the main property of most of the instances in the bag. While instances don't have label.
	The goal of MIL is to predict the label of each bag through the unlabeled instances in it.

**Bag labels** are automatically created through existing knowledge databases(KBs).

### Training
#### Vision-language pre-training (VLP) models

Goal
	To understand the semantic interactions in each image of the bag, 
	then to extract commonsense facts about a pair of query entities 
#### Contrastive attention mechanism model

Goal
	To select informative pair of query entities,
	to select meaningful images,
	so that to summarize bag-level commonsense relations

# Database

[Visual genome](https://homes.cs.washington.edu/~ranjay/visualgenome/api.html) for Commonsense knowledge acquisition(VG-CKE) dataset
ConceptNet

# Reference

**Common knowledge**
	[What Is a Knowledge Representation?](https://ojs.aaai.org/aimagazine/index.php/aimagazine/article/view/1029)

**Commonsense knowledge bases (KBs)**
	[ConceptNet—a practical commonsense reasoning tool-kit](https://link.springer.com/article/10.1023/B:BTTJ.0000047600.45421.6d)
	[ConceptNet 5.5: An Open Multilingual Graph of General Knowledge](https://ojs.aaai.org/index.php/AAAI/article/view/11164)
	[ATOMIC: An Atlas of Machine Commonsense for If-Then Reasoning](https://ojs.aaai.org/index.php/AAAI/article/view/4160)

**Previous work on Commonsense knowledge extraction (CKE)**
	From plain text
		[Commonsense Knowledge Base Completion](https://aclanthology.org/P16-1137.pdf)
		Pre-trained language models (PLMs)
			[Language Models as Knowledge Bases?](https://arxiv.org/abs/1909.01066)
			[COMET: Commonsense Transformers for Automatic Knowledge Graph Construction](https://arxiv.org/abs/1906.05317)
			Problems
				Obvious commonsense is rarely reported in text
				[Reporting bias and knowledge acquisition](https://dl.acm.org/doi/abs/10.1145/2509558.2509563)
				[The World of an Octopus: How Reporting Bias Influences a Language Model's Perception of Color](https://arxiv.org/abs/2110.08182)
				Commonsense in PLMs suffers from low consistency and significant reporting bias 
				[Do neural language mod- els overcome reporting bias?](https://aclanthology.org/2020.coling-main.605/)
				[Evaluating commonsense in pre-trained language models](https://ojs.aaai.org/index.php/AAAI/article/view/6523)
				[Measuring and improving consistency in pretrained language models](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00410/107384/Measuring-and-Improving-Consistency-in-Pretrained)
		Doubt whether learning purely from the surface text forms can lead to real understanding of commonsense meaning
			[Climbing towards NLU: On Meaning, Form, and Understanding in the Age of Data](https://aclanthology.org/2020.acl-main.463/)
	From visual perceptions
		83% of the triplets in visual relation learning datasets cannot be found in ConceptNet
		Existing image-based CKE methods
			Confined to restricted interaction types
			[NEIL: Extracting visual knowledge from web data](https://openaccess.thecvf.com/content_iccv_2013/html/Chen_NEIL_Extracting_Visual_2013_ICCV_paper.html)
			[Acquiring common sense spatial knowledge through implicit spatial templates](https://ojs.aaai.org/index.php/AAAI/article/view/12239)[Automatic Extraction of Commonsense LocatedNear Knowledge](https://arxiv.org/abs/1711.04204)
			Required extensive human annotation
			[Learning common sense through visual abstraction](https://openaccess.thecvf.com/content_iccv_2015/html/Vedantam_Learning_Common_Sense_ICCV_2015_paper.html)

**CLEVER model**
	Distantly Supervised multi-instance learning problem
	[Solving the multiple instance problem with axis-parallel rectangles](https://www.sciencedirect.com/science/article/pii/S0004370296000343)
	DS problem
	[[Contrastive Instance Learning Framework for Distantly Supervised Relation Extraction|CIL]]

distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \ && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \ && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list