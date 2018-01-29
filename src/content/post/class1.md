+++
date = "26 Jan 2018"
draft = true
title = "Class 1: Intro to Adversarial Machine Learning"
author = "Team Panda"
+++
# Outline of Class 1 blog

## Machine Learning Background
- Other ML algorithms use linear decision boundaries (SVM, LR, ...)
- Deep learning uses linear units with nonlinear composition
	- More susceptible to attacks
- Define terms like gradient and loss

---
## Applications and Vulnerabilities
	
To get a better idea of what security and privacy mean in a machine learning context, we highlight a few
applications and examine the possible consequences of vulnerabilities in these domains.

One of the most commonly discussed applications is in self-driving cars, which use machine learning algorithms for image recognition tasks, such as identifying traffic signs and road hazards.  If an attacker were to cause an erroneous classification, e.g. mistaking a stop sign for a speed limit sign or missing a pedestrian in a crosswalk, both property and human lives would be in jeopardy. (Although luckily self-driving cars use a variety of other techniques to avoid collisions like GPS and radar.)  

Machine learning has also found a place in the medical world, being used to identify patterns and trends in patient histories that can be used for diagnoses, to recognize abnormalities in medical imaging, and even to evaluate the efficiency of a hospital's workflow.  In healthcare, the risks are two-fold - there are both safety and privacy risks that need to be considered.  An attacker could manipulate the models used for diagnostics and pattern recognition, thereby putting patient health at risk, and there is also the potential for confidential patient data to be leaked either through the training process or the final model itself. 

There are numerous other uses of ML today (targeted advertising, fraud detection, malware detection, etc.) and each carries with it unique security and privacy concerns.

---
## Adversarial Fundamentals

In machine learning, an adversarial example is an input to the trained model that has been manipulated so that the model returns a different, incorrect output.  A classic adversarial example for image classification is from a paper by Goodfellow et. al., which features a photo of a panda that was originally classified correctly, but is misclassified when carefully-crafted noise is added.  The model classifies the new image as a gibbon, even though human eyes can easily see it is a panda. 

<p align="center">
<img src="/images/panda.png" width="600" >
</p>

### Classifying Attacks

Attacks can be categorized by many different characteristics, but are often referred to in terms of the attack method, the goal of the attack, and the adversary's level of knowledge.

#### Attack Method

A poisoning attack is when the adversary adds carefully-crafted samples into the training data, thereby disrupting the learning process and manipulating the final trained model.  An evasion attack occurs when a sample originally correctly classified into class A is manipulated and fed back into the model, now receiving an output that is *not* class A.  A key difference between these two attacks is where each occurs in the ML pipeline, with poisoning occuring before the training stage and evasion attacks occuring after training, during the testing stage.

<p align="center">
<img src="/images/ml_pipeline.jpg" width="650" >
</p>

#### Goal of Attack

The adversary's goal is another way to describe attacks, which in this case means whether or not there was a specific goal classification or not.  Error-generic (also called untargeted) attacks aim to misclassify a given sample, but do not have a specifically targeted class. Conversely, error-specific (targeted) attacks deliberately change the sample's classification from the original class A to a chosen class B. 

#### Adversary Knowledge

The level of knowledge the adversary possesses generally falls into one of three loosely defined categories: black box, grey box, and white box attacks.  Black box attacks assume the lowest level of adversary knowledge, generally only allowing the final classification to be known without any finer details about the model.  On the other end of the spectrum, white box attacks assume that the attacker knows everything about the model, including the specific algorithm used, the feature set, the training data, etc. An attack by an adversary who knows more about the model than a black box but less than a white box is called a grey box attack, although the exact definition tends to vary.

<p align="center">
<img src="/images/boxes.png" width="600" >
</p>


---
## Goodfellow's Explaining and Harnessing Adversarial Examples
You can find the [paper](https://arxiv.org/abs/1412.6572) here.
#### Potemkin village
- Freshly painted outside
- Decaying inside/back

#### FGSM
- Take opposite direction of gradient loss
- Reverse of optimization

#### ReLU vs Sigmoid

---
## Adversarial training
#### Effects of nonlinearities
- More capacity leads to vulnerability

#### Solutions
- Mixture of original and adversarial examples
- Add an adversarial regularizer

---
## Transferability of adversarial samples
- Cross-evasion
- Intra-technique

#### Learning classifier substitutes
- Use smaller number of queries/training data

#### Steal model by query
#### Adversarial sample crafting


--- Team Panda
