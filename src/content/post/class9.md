+++
date = "30 Mar 2018"
draft = true
title = "Class 9: Adversarial Malware Detection"
author = "Team Nematode"
slug = "class9"
+++


### Introduction

### Malware History - Machine Learning Origins

### PDF Malware Classifiers

### Hidost: A Static Machine-Learning-Based Detector of Malicious Files

### Machine Learning on Malware is Hard
While machine learning algorithms have been successfully applied in various vision and natural language related tasks, they have not been fully successful in malware detection. The reasons are mainly three-fold:

#### High Cost of Error
In the case of malware detection, when measuring the performance of a classifier, both false positive rate and false negative rate are important factors. While the false positive rate means a benign software is classified as a malware, in the false negative case a malware is classified as benign. Both the events are undesirable for malware detection. Since, false positive requires excessive spending of a human analyst's time which could be expensive and time consuming. On the other hand, false negative means a malware is left undetected, which can have serious security consequences. A typical machine learning model trained for the malware detection task tends to have non-zero false positive rates and false negative rates.

#### Semantic Gap
There is a disconnect between the prediction result of a machine learning model and the action to be taken based on the result. For instance, if a model predicts a malware with 65% confidence, what action should be taken? Should the target software be removed without intimation to the end user? Should the user be alerted for a manual inspection? In such a scenario, it is hard to interpret the results and take a meaningful action.

#### Difficulty with Evaluation
One of the major difficulty in the evaluation of malware detection tool is the availability of datasets. Most of the public datasets like Contagio or Drebin are small or outdated, whereas the malwares keep updating aggressively. It is difficult to create new rich malware datasets due to data privacy concerns and the labelling of malwares in the wild. This poses problem for training and evaluating the machine learning models.

Another difficulty is the evasiveness of the malwares. Malwares have become dynamic enough to evade the malware classifiers. Given a white-box access to the classifier, malware can perform adversarial training like gradient-based method to evade detection. Even with black-box access, the malware can perform mimicry attack by appending features of benign samples. However, this may or may not break the malware's functionality and hence it is hard to generate useful samples. One variant is to use [reverse-mimicry attack](https://dl.acm.org/citation.cfm?id=2484327) by embedding malicious features into benign file to generate malicious samples. Recent literature has also shown evasion by [genetic mutation](http://people.cs.vt.edu/~gangwang/class/cs6604/papers/ndss16.pdf) and [hill climbing method](https://acmccs.github.io/papers/p119-dangA.pdf), where the malware can dynamically adapt to evade detection

### State of the Art

### Future of the Field

### Conclusion

We've explored the current state of adversarial malware detection including Hidost. From simple to metamorphic viruses, malware has evolved into increasingly sophisticated threats. The field of adversarial malware detection has been evolving as well, as methods are developed that are increasingly effective at detecting malicious code. As the cutting edge of malware detection moves forward, we will see new fronts open up in the contest between malicious files and the algorithms that detect them.

### Sources
> Liang Tong, Bo Li, Chen Hajaj, Chaowei Xiao, Yevgeniy Vorobeychik. 2017. Hardening Classifiers against Evasion: the Good, the Bad, and the Ugly.  arXiv:1708.08327. Retrieved from https://arxiv.org/abs/1708.08327v2 [[PDF]] (https://arxiv.org/abs/1708.08327v2)

> R. Sommer and V. Paxson, "Outside the Closed World: On Using Machine Learning for Network Intrusion Detection," 2010 IEEE Symposium on Security and Privacy, Oakland, CA, USA, 2010, pp. 305-316.
https://doi.org/10.1109/SP.2010.25 [[PDF]] (http://ieeexplore.ieee.org/document/5504793/)
