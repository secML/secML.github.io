+++
date = "09 Feb 2018"
draft = false
title = "Class 3: Adversarial Machine Learning"
author = "Team Gibbon"
slug = "class3"
+++

This week’s topic was once again adversarial machine learning. The underlying problem is that machine learning techniques assume that training and testing data are generated from the same distribution. Therefore, adversaries can choose inputs to exploit the algorithms by manipulating data. We began class by discussing common distance metrics (\\( L_0, \, L_2, \, L\infty \\)), standard datasets, and the history of adversarial ML. However, the main theme was defense techniques can be used safely to prevent adversarial attacks. Below we discuss four papers that discuss both effective and ineffective defenses.

## Defensive Distillation

## Evaluating Robustness of Neural Networks

## Obfuscated Gradients

## Resistance to Adversarial Attacks

> Aleksander Madry, Aleksandar Makelov, Ludwig Schmidt, Dimitris Tsipras, and Adrian Vladu. _Towards Deep Learning Models Resistant to Adversarial Attacks_.  ICLR 2018. [[PDF](https://arxiv.org/pdf/1706.06083.pdf)]

[Madry et al.](https://arxiv.org/pdf/1706.06083.pdf) propose a general framework to study the defense of deep learning models against adversarial attacks.  The goal of their framework is to provide precise security guarantees about how a particular defense stacks up against entire classes of attacks.  Specifically, they offer the following saddle point problem:

$$\underset{\theta}{\text{min}} \, \rho(\theta), \quad \text{where} \quad \rho(\theta) = \mathbb{E}_{(x, y) \sim \mathcal{D}} \bigg[ \underset{\delta \in \mathcal{S}}{\text{max}} \, L(\theta, x + \delta, y) \bigg] \, .$$

Let’s break this down:

* \\( \theta \\) is the set of parameters of the model.  The attacker aims to exploit an existing model with a predetermined \\( \theta \\), and the defender can tune \\( \theta \\) as they wish.
* \\( x \\) and \\( y \\) are the example and label.
* \\( L \\) is the loss function.
* \\( \mathbb{E} \\) is a risk function.

This is a classic saddle point problem, consisting of two connected parts: an inner maximization problem, and an outer minimization problem. An adversary seeks to find an example that maximizes the model’s loss (the inner problem), and the defender seeks to tune their model such that the potential loss is minimized (the outer problem).  Therefore, solving this problem (by minimizing rho) maximizes the robustness of a deep learning model.

The inner loss function was modeled with a fast gradient sign method (FGSM) attack. The FGSM perturbed data was used for as training data on the defense side. The outer minimization function was a little more complicated. It’s not simple enough to train on FGSM adversaries. We want something that’s universally robust, so we use PGD, a first-order method to solve constrained optimization problems. The authors used PGD to find local maxima, since they are the areas of highest loss.

![](https://raw.githubusercontent.com/azvchen/secML.github.io/master/src/content/images/class3/Madry1.png)

These graphs show how the loss scales as the number of PGD iterations increases.  The loss grows and plateaus as the number of iterations grows (as expected for any model).  However, the adversarially-trained models display much less loss than the naturally-trained models, indicating that Madry et. al.’s defense works well against their attacks.

![](https://raw.githubusercontent.com/azvchen/secML.github.io/master/src/content/images/class3/Madry2.png)

These graphs show the distributions of loss values, for various starting points.  The loss values are obtained by randomly sampling points within an \\(L\infty\\)-ball centered at the starting point, then using PGD to find the local maxima near those random points.  Although the locations of the points are widely distributed, the loss values are all clustered together.  The authors claim that these graphs display the universality of PGD: any local maxima with significantly higher losses would likely be infeasible for any other first-order method to find.

— Team Gibbon: \\
Austin Chen, Jin Ding, Ethan Lowman, Aditi Narvekar, Suya

## References

[[1]](https://www.cs.cornell.edu/~shmat/shmat_oak17.pdf) A. Madry, A. Makelov, L. Schmidt, D. Tsipras, A. Vladu, "Towards Deep Learning Models Resistant to Adversarial Attacks".  February 2018.
