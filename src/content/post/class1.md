+++
date = "26 Jan 2018"
draft = false
title = "Class 1: Intro to Advarsarial Machine Learning"
author = "Team Panda"
+++
# Outline of Class 1 blog

## Machine Learning Background
- Other ML algorithms use linear decision boundaries (SVM, LR, ...)
- Deep learning uses linear units with nonlinear composition
	- More susceptible to attacks
- Define terms like gradient and loss

---
## Applications and Vulnabilities
- Workflow patterns
- Attack examples
	- Workflow disruption by a foriegn power
	- Harming medical diagnosis

---
## Poisoning and Evasion
#### Error generic vs error specific
TODO: Include table from slides 

---
## White, grey, and black boxes
- Comparison of different attack visibilities

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
