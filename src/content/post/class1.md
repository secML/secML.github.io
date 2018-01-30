+++
date = "26 Jan 2018"
draft = false
title = "Class 1: Intro to Advarsarial Machine Learning"
author = "Team Panda"
+++
# Outline of Class 1 blog

## Machine Learning Background
<!-- - Other ML algorithms use linear decision boundaries (SVM, LR, ...)
- Deep learning uses linear units with nonlinear composition
	- More susceptible to attacks
- Define terms like gradient and loss -->


In Machine Learning, we train our model with training data along with the label associated with it. For training purposes, we extract the features and based on that we deploy an algorithm to train the whole model. For classifying the testing data, classifier uses decision boundary to separate points of the data belonging to each class. In a statistical-classification problem with two classes, a decision boundary partitions all the underlying vector space into two separate classes. It will classify all the points on one side of the decision boundary keeping the rest points on the other side of the boundary. Like this way it will separate all the points into two different classes. SVM-support vector machine is a linear classifier which construct a boundary by focusing two closest points from different classes and it finds the line that is equidistant to these two points.

#### Loss functions
Loss functions defines that how good our model is at making predictions for a given scenario. It has its own curve and its own gradients. The slope of the curve indicates the appropriate way of updating the parameters to make the model more accurate in case of prediction. A frequently used loss function is the 0-1 loss function.

<p align="center">
<img src="/images/01_loss_function.png" width="150" >
</p>


where I is the indicator notation. Hinge loss function is also popular in machine learning field. It provides a relatively tight, convex upper bound on the 0-1 indicator function.

<!-- <p align="center">
<img src="/images/hinge_function.png" width="150" >
</p> -->

#### Gradient Descent
Gradient Descent is an optimization algorithm which is used to minimize cost function by iteratively moving the direction of the steepest descent. In machine learning, we use gradient descent to update the parameters (coefficients in Linear Regression and weight in neural networks) of our model. 

<p align="center">
<img src="/images/gradient_descent_graph.png" width="300" >
</p>

In the figure, weight is slowly decreasing from the initial stage. J(min) is the minimum value of the cost function which can be obtained by optimizing the algorithm. 

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
